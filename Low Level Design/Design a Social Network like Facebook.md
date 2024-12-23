
### Requirements

#### User Registration and Authentication:

- Users should be able to create an account with their personal information, such as name, email, and password.
- Users should be able to log in and log out of their accounts securely.

#### User Profiles:

- Each user should have a profile with their information, such as profile picture, bio, and interests.
- Users should be able to update their profile information.

#### Friend Connections:

- Users should be able to send friend requests to other users.
- Users should be able to accept or decline friend requests.
- Users should be able to view their list of friends.

#### Posts and Newsfeed:

- Users should be able to create posts with text, images, or videos.
- Users should be able to view a newsfeed consisting of posts from their friends and their own posts.
- The newsfeed should be sorted in reverse chronological order.

#### Likes and Comments:

- Users should be able to like and comment on posts.
- Users should be able to view the list of likes and comments on a post.

#### Privacy and Security:

- Users should be able to control the visibility of their posts and profile information.
- The system should enforce secure access control to ensure data privacy.
#### Notifications:

- Users should receive notifications for events such as friend requests, likes, comments, and mentions.
- Notifications should be delivered in real-time.

#### Scalability and Performance:

- The system should be designed to handle a large number of concurrent users and high traffic load.
- The system should be scalable and efficient in terms of resource utilization.

## Classes, Interfaces and Enumerations

1. The **User** class represents a user in the social networking system, containing properties such as ID, name, email, password, profile picture, bio, list of friends, and list of posts.

```
public class User {

	private final Long userId;
	private final String name;
	private final String email;
	private final String password;
	private final String profilePicUrl;
	private final String bio;
	private final List<User> friends;
	private final List<Post> posts;

	public User(Long userId, String name, String email, String password, String bio) {
		this.userId = userId;
		this.name = name;
		this.email = email;
		this.password = password;
		this.bio = bio;
		this.friends = new ArrayList<>();
		this.posts = new ArrayList<>();
	}

	public void setProfilePicUrl(String profilePicUrl) {
		this.profilePicUrl = profilePicUrl;
	}

	public void addFriend(User friend) {
		this.friends.add(friend);
	}

	public void addPost(Post post) {
		this.posts.add(post);
	}

	// Getters
	
}
```

2. The **Post** class represents a post created by a user, containing properties such as ID, user ID, content, image URLs, video URLs, timestamp, likes, and comments.

```
public class Post {

	private final Long postId;
	private final Long userId;
	private final String content;
	private final List<String> imageUrls;
	private final List<String> videoUrls;
	private final Timestamp timestamp;
	private final List<String> likes;
	private final List<Comment> comments;

	public Post(Long postId, Long userId, String content, List<String> imageUrls, String videoUrl, Timestamp timestamp) {
		this.postId = postId;
		this.userId = userId;
		this.content = content;
		this.imageUrls = imageUrls;
		this.videoUrls = videoUrls;
		this.timestamp = timestamp;
		this.likes = new ArrayList<>();
		this.comments = new ArrayList<>();
	}

	public void addLike(Like like) {
		this.likes.add(like);
	}

	public void addComment(Comment comment) {
		this.comments.add(comment);
	}

	public void setContent(String content) {
		this.content = content;
	}

	public void setImageUrl(String imageUrl) {
		this.imageUrl = imageUrl;
	}

	public void setVideoUrl(String videoUrl) {
		this.videoUrl = videoUrl;
	}

	// Getters
}
```

3. The **Comment** class represents a comment made by a user on a post, containing properties such as ID, user ID, post ID, content, and timestamp.

```
public class Comment {

	private Long commentId;
	private Long userId;
	private Long postid;
	private String content;
	private Timestamp timestamp;

	public Comment(Long commentId, Long userId, Long postId, String content, Timestamp timestamp) {
		this.commentId = commentId;
		this.userId = userId;
		this.postId = postId;
		this.content = content;
		this.timestamp = timestamp;
	}

	public void setContent(String content) {
		this.content = content;
	}

	// Getters
}
```

4. The **Notification** class represents a notification generated for a user, containing properties such as ID, user ID, notification type, content, and timestamp.

```
public class Notification {

	private Long id;
	private Long userId;
	private NotificationType type;
	private String content;
	private Timestamp timestamp;

	public Notification(Long id, Long userId, NotificationType type, String content, Timestamp timestamp) {
		this.id = id;
		this.userId = userId;
		this.type = type;
		this.content = content;
		this.timestamp = timestamp;
	}

	public void setContent(String content) {
		this.content = content;
	}

	// Getters
}
```

5. The **NotificationType** enum defines the different types of notifications, such as friend request, friend request accepted, like, comment, and mention.

```
public enum NotificationType {
	LIKE, 
	COMMENT,
	MENTION,
	FRIEND_REQUEST,
	FRIEND_REQUEST_ACCEPTED
}
```

6. The **SocialNetworkingService** class is the main class that manages the social networking system. It follows the Singleton pattern to ensure only one instance of the service exists.

The SocialNetworkingService class provides methods for user registration, login, profile updates, friend requests, post creation, newsfeed generation, likes, comments, and notifications.

```
public class SocialNetworkingService {
	private static SocialNetworkingService instance;
	private final Map<Long, User> users;
	private final Map<Long, Post> posts;
	private final Map<Long, List<Notification>> notifications;
	
	private SocialNetworkingService() {
		users = new ConcurrentHashMap<>();
		posts = new ConcurrentHashMap<>();
		notifications = new ConcurrentHashMap<>();
	}

	public static synchronized SocialNetworkingService getInstance() {
		if(instance == null) {
			 instance = new SocialNetworkingService();
		}
		return instance;
	}

	public void registerUser(User user) {
		users.put(user.getId(), user);
	}


	public User login(String email, String password) {
		for(User user : users.values()) {
			if(user.getEmail().equals(email) && user.getPassword().equals(password)) {
				return user;
			}
		}
		return null;
	}

	public void updateUserProfile(User user) {
		users.put(user.getId(), user);
	}

	public void sendFriendRequest(Long senderId, Long receiverId) {
		User receiver = users.get(receiverId);
		if(receiver != null) {
			Notification notification = new Notification(generateNotificationId(), receiverId, NotificationType.FRIEND_REQUEST, "Friend request from " + senderId, new Timestamp(System.currentTimeMillis()));
			addNotification(receiverId, notification);
		}
	}

	public void acceptFriendRequest(String userId, String friendId) {
		User user = users.get(userId);
		User friend = users.get(friendId);
		if(user != null && friend != null) {
			user.getFriends().add(friendId);
			friend.getFriends().add(userId);
			Notification notification = new Notification(generateNotificationId(), friendId,                     NotificationType.FRIEND_REQUEST_ACCEPTED, "Friend request accepted by " + userId, new Timestamp(System.currentTimeMillis()));
			addNotification(friendId, notification);
		}
	}

	public void createPost(Post post) {
		posts.put(post.getId(), post);
		User user = users.get(post.getUserId()); 
		if(user != null) {
			user.getPosts().add(post);
		}
	}

	public List<Post> getNewsfeed(String userId) {
		List<Post> newsfeed = new ArrayList<>();
		User user = users.get(userId);
		if(user != null) {
			List<Long> friendIds = user.getFriends();
			for(Long friendId : friendIds) {
				User friend = users.get(friendId);
				if(friend != null) {
					newsfeed.addAll(friend.getPosts());
				}
			}
			newsfeed.addAll(user.getPosts());
			newsfeed.sort((p1, p2) -> p2.getTimestamp().compareTo(p2.getTimestamp()));
		}
		return newsfeed;
	}

	public void likePost(String userId, String postId) {
		Post post = posts.get(postId);
		if(post != null && !post.getLikes().contains(userId)) {
			post.getLikes().add(userId);
			Notification notification = new Notification(generateNotificationId(),post.getUserId() ,NotificationType.LIKE, "Your post was liked by "+ userId, new Timestamp(System.currentTimeMillis()));
			addNotification(post.getUserId(), notification);
		}
	}

	public void commentOnPost(Comment comment) {
		Post post = posts.get(comment.getPostId()); 
		if(post != null) {
			post.getComments().add(comment);
			 Notification notification = new  Notification(generateNotificationId(), post.getUserId(),
                    NotificationType.COMMENT, "Your post received a comment from " + comment.getUserId(),
                    new Timestamp(System.currentTimeMillis()));
            addNotification(post.getUserId(), notification);        
		}
	}

	private void addNotification(String userId, Notification notification) {
		notifications.computeIfAbsent(userId, k -> new CopyOnWriteArrayList<>()).add(notification);
	}

	public List<Notification> getNotifications(String userId) {
		notifications.getOrDefault(userId, new ArrayList<>());
	}

	private String generateNotificationId() {
		return UUID.randomUUID().toString();
	}
}
```

7. The **SocialNetworkingDemo** class demonstrates the usage of the social networking system by registering users, logging in, sending friend requests, creating posts, liking posts, commenting on posts, and retrieving newsfeed and notifications.

```
public class SocialNetworkingDemo {

	public static void main(String[] args) {
			SocialNetworkingService socialNetworkingService = SocialNetworkingService.getInstance();
	  // User registration
        User user1 = new User(1, "John Doe", "john@example.com", "password", "profile1.jpg", "I love coding!", new ArrayList<>(), new ArrayList<>());
        User user2 = new User(2, "Jane Smith", "jane@example.com", "password", "profile2.jpg", "Exploring the world!", new ArrayList<>(), new ArrayList<>());
        socialNetworkingService.registerUser(user1);
        socialNetworkingService.registerUser(user2);

		 // User login
        User loggedInUser = socialNetworkingService.loginUser("john@example.com", "password");
        if (loggedInUser != null) {
            System.out.println("User logged in: " + loggedInUser.getName());
        } else {
            System.out.println("Invalid email or password.");
        }

		// Send friend request
        socialNetworkingService.sendFriendRequest(user1.getId(), user2.getId());

        // Accept friend request
        socialNetworkingService.acceptFriendRequest(user2.getId(), user1.getId());

		        // Create posts
        Post post1 = new Post(1, user1.getId(), "My first post!", new ArrayList<>(), new ArrayList<>(), new Timestamp(System.currentTimeMillis()), new ArrayList<>(), new ArrayList<>());
        Post post2 = new Post(2, user2.getId(), "Having a great day!", new ArrayList<>(), new ArrayList<>(), new Timestamp(System.currentTimeMillis()), new ArrayList<>(), new ArrayList<>());
        socialNetworkingService.createPost(post1);
        socialNetworkingService.createPost(post2);


	        // Like a post
        socialNetworkingService.likePost(user2.getId(), post1.getId());

        // Comment on a post
        Comment comment = new Comment(1, user2.getId(), post1.getId(), "Great post!", new Timestamp(System.currentTimeMillis()));
        socialNetworkingService.commentOnPost(comment);


		// Get newsfeed
        List<Post> newsfeed = socialNetworkingService.getNewsfeed(user1.getId());
        System.out.println("Newsfeed:");
        for (Post post : newsfeed) {
            System.out.println("Post: " + post.getContent());
            System.out.println("Likes: " + post.getLikes().size());
            System.out.println("Comments: " + post.getComments().size());
            System.out.println();
        }

        // Get notifications
        List<Notification> notifications = socialNetworkingService.getNotifications(user1.getId());
        System.out.println("Notifications:");
        for (Notification notification : notifications) {
            System.out.println("Type: " + notification.getType());
            System.out.println("Content: " + notification.getContent());
            System.out.println();
        }

	} 
}
```


### References

https://github.com/ashishps1/awesome-low-level-design/blob/main/problems/social-networking-service.md

