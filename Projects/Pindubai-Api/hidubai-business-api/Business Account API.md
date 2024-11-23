
1. Get Business Account Users:
		URI : /accounts/{businessAccountId}/users
		Roles : ROLE_BUSINESS_ACCOUNT_ADMIN, ROLE_BUSINESS_ACCOUNT_VIEWER
		1. Validate business account id.
		2. Check if user is authenticated.
		3. Call list users method in business account service.
			1. Fetch the business account using business account id.
			2. Check if business user has access to edit the business account. If yes, get the user list from the business account.
			3. Check if business user has access to view the business account. If yes, get all the business users of auth user related business
		4. Get total business count using business account id.
		5. Check if authenticated user has the permission to edit the business account.
		6. Convert the domain to BusinessAccountUsersResponse and send back the response.
		
2. Get account businesses:
		URI : /accounts/{businessAccountId}/businesses
		Roles : ROLE_BUSINESS_ACCOUNT_ADMIN
		1. Validate business account id.
		2. Check if user is authenticated.
		3. Call the list businesses method of business account service.
			1. Check if the user is account admin if no, throw exception.
			2. Fetch the business account using business account id.
			3. If the authenticated user is account admin:
				1. Get the list of all the local businesses using business account id.
				2. return the list of local business of non null objects.
			4. If the authenticated user is account viewer:
				1. Get the business user to business role list whose having role -ROLE_BUSINESS_ADMIN.
				2. Convert the filtered roles into list of business info domain.
			5.  Return the list of business info domain whichever condition satisfy 3 or 4.
		4. Sort the business info domain list by business name and convert it into business info response list and return the list.

