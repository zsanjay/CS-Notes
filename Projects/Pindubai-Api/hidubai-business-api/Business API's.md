
1. Business Claim API:
	URL -  https://bizapi-qa.hidubai.com/v1/biz/businesses/58790731e4b0c0742c517a98/claim
	 Only who has ROLE_BUSINESS_USER, ROLE_BUSINESS_ADMIN and not having 
	 ROLE_BUSINESS_ACCOUNT_VIEWER role can claim the business.

	 Request Body :	  
		{
			"designation" : "Owner"
		}
	 Header - 
		Authorization : eyJraWQiOiJRZ3hcLzlIRUtib1pNU3NSRFB3KzJRZFhqTkxCUmFiYTZ4R1Y4VXhkNnRvQT0iLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIxMTRiZTA2NC0zNjk1LTQxMzMtODIzNC1mYTczZTllOWI0ZjciLCJjb2duaXRvOmdyb3VwcyI6WyJidXNpbmVzc19hY2NvdW50X2FkbWluIiwiYnVzaW5lc3NfYWRtaW4iLCJidXNpbmVzc191c2VyIl0sImlzcyI6Imh0dHBzOlwvXC9jb2duaXRvLWlkcC5hcC1zb3V0aGVhc3QtMS5hbWF6b25hd3MuY29tXC9hcC1zb3V0aGVhc3QtMV8wYnFjS2lrdWsiLCJjbGllbnRfaWQiOiIxbWpuZTc3NzJoZjg1bTJvOWtxa2Ewc3Q5dSIsIm9yaWdpbl9qdGkiOiI4ZTdhNDQ1OS00OTcyLTRhMzQtYWMzNi0zZjA1MjJjYWJkMmQiLCJldmVudF9pZCI6ImExNWQ2ZTljLTM1M2MtNGFkNC04ZjIyLTgzOGEzMmU4Mzk5YiIsInRva2VuX3VzZSI6ImFjY2VzcyIsInNjb3BlIjoiYXdzLmNvZ25pdG8uc2lnbmluLnVzZXIuYWRtaW4iLCJhdXRoX3RpbWUiOjE3MTUwNTc3NDksImV4cCI6MTcxNTA1OTU0OSwiaWF0IjoxNzE1MDU3NzQ5LCJqdGkiOiJkODgxMDY1My1iNjhiLTQyNDYtOGE3ZS0yZjAyZGFhZTdiNzIiLCJ1c2VybmFtZSI6InNuMTcwMDU1NjUxOTcwMyJ9.EJvWrYPzVsslnGnEZrB5dp03RFHZiZ4X9UdejCTI9ToDWqpVzRHkZM5d7UvxCh3wui5JA-WSKWOfkE_V8IYL0_-dO0fTO3NEU3-_vqGn2IABEYPfEoQscLkbVIU5RvytkoMdQTsL-uviiFU2EKdnvTvE4t8Zn2iTe4buB-LrH_YzLAXe1mlMpcH_4JpTam_USoI7J7pn0Dh6A7gOM8_npnYPPKuJuL_xYoQFo5Ku2d9eJaelUdqzbf0Wd-lk0yfh96PTvZuoWZDGDIcLn2r-tf8pzZh0BE1zp5Wu5xkYl9G27rLbYKfQ_NX1sspQ80yHAVAV4s2R9QD86-lkiAx33A


	BusinessClaimService

		claimBusiness method:
		1. To claim the business first get the localbusiness using the businessId from the Business Service.
		2. Get business user domain by token from the Auth Service.
		3. Get business user entity using the username from the business user service.
		4. Check Business Claim Eligibility:
			1. Check if business is already claimed businessAccount != null.
			2. Check if business is closed LocalBusinessStatus = DISABLED or LocalBusinessState = CLOSE.
			3. Check if business claim request is already pending using localbusiness and BusinessClaimStatus.NEW from the BusinessClaimPersistentRepository.
			4. Check if business user is not new and business user is not having business account admin role then access is denied.
		5. Create a business claim entity using BusinessClaimEntityAssembler.toEntity method.
		6. Save the business claim entiy using businessClaimPersistentRepository.
		7. Get the REPORT_BUSINESS_CLAIM_V2 report type from the db and create a new report with status PENDING.
				`{`
				`"_id" : ObjectId("6525554c18456c634e7ca482"),`
				
				`"_class" : "com.pindubai.api.report.type.model.ReportType",`
				
				`"code" : "REPORT_BUSINESS_CLAIM_V2",`
				
				`"prettyCode" : "Business claim V2",`
				
				`"context" : "CLAIM",`
				
				`"reportSet" : "REPORT",`
				
				`"label" : {`
				
				`"ar" : "طلب الادعاء الأعمال",`
				
				`"en" : "Business Claim Request"`
				
				`},`
				
				`"description" : "[$USERNAME$] submitted a claim request for the business [$BUSINESSNAME$].",`
				
				`"createdBy" : "startup-admin",`
				
				`"lastModifiedBy" : "fazil.qa.ap@yopmail.com",`
				
				`"createdDate" : ISODate("2023-10-10T13:44:44.664+0000"),`
				
				`"lastModifiedDate" : ISODate("2023-10-12T12:38:56.129+0000"),`
				
				`"uuid" : "57744b12-b7ff-4362-80db-850b6e99c54b"`
				
				`}`
		8. Save the report using report service.
		9. Get Auto Approval Eligilbility.
			1. Check if isBusinessClaimedInHdcom - Business already claimed in HiDubai public portal.
			2. Check if Users Mobile Verified.
			3. Check if Users Email Verified and email matching.
			4. Check if Users Email Verified and domain matching.
		10. Approve Business Claim if it is eligible for auto approval.
		11. Get the Business Claim Response using business domain to response assembler.
		12. Send confirmation mail (message = "Submitted" and status = BusinessClaimStatus.NEW) if report status is pending.
		13. Or else update cognito admin user group and token stale time with message  = "Approved" and status = BusinessClaimStatus.APPROVED.
		14. Send back the business claim response.


2.  Get local business using trade license filter:
		
		URL -  https://bizapi-qa.hidubai.com/v1/biz/businesses
		 Role - (ROLE_BUSINESS_USER or ROLE_BUSINESS_ADMIN or ROLE_BUSINESS_VIEWER) and not have ROLE_BUSINESS_ACCOUNT_VIEWER.
		 1. Check if the user is authenticated using getAuthenticatedUser method which checks the token and get the username and check the corresponding username in the db.
		 2. Convert the request of filter to domain using RequestToDomainAssembler and get BusinessSearchFilter domain.
		 3. Call the business service to get the business by filter.
		 4. Call the Local Business Repository findByTradeLicenseAndBusinessAccountIsNull method and pass the sort object to sort "lastModifiedDate" by desc order. Here business account is null means business is not claimed.
		 5. Call the getDesiredBusinessList method and pass the list of localbusinesses.
		 6. Convert the local businesses entity into local businesses domain.
		 7. Return the response by converting the domain into response.
	

3. Get business detail using business id:
		URI : /biz/businesses/58790731e4b0c0742c517a98
		Role : ROLE_BUSINESS_USER, ROLE_BUSINESS_ADMIN , ROLE_BUSINESS_VIEWER
		 1.  Validate Business Id
		 2. Check if the user is authenticated or not.
		 3. Call the getBusinessByBusinessId from the business service.
			 1. Get the local business using getLocalBusinessByBusinessId method.
			 2. Check the business user role can view  the business.
			 3. Convert the local business to local business domain.
			 4. Get the business view count from the AnalyticsEventRepository countByEntityIdAndTypeCodeAndEntityType method and set the in localbusiness domain.
			 5. Add user permission with the response.

4. Update business:
		Patch Method
		URI: /biz/businesses/58790731e4b0c0742c517a98
		Role: ROLE_BUSINESS_ADMIN
		Request Body : BusinessPatchRequest
		1. Check if user is authenticated.
		2. Convert BusinessPatchRequest to domain.
		3. Call patchLocalBusiness method  in business service
			1. Get the existing local business entity using business Id.
			2. Check if business user has the permission to update the business setting of that particular business.
			3. Validate location of the business is dubai or not using the Geo Location Service  isDubaiAddress method.
			4. Convert the business domain to business entity
			5. checkAndSetComplex , checkAndSetBuilding , checkAndSetLandmarks , checkAndSetNeighborhood.
			6. check if phoneNumber modified and address modified.
			7. After all update checks save the local Business Entity.
			8. Publish the after save event.

	5. Get Notifications by business id.
		URI: /biz/businesses/58790731e4b0c0742c517a98/notifications
		Role: ROLE_BUSINESS_ADMIN , ROLE_BUSINESS_VIEWER
		1. Check if user is authenticated.
		2. Validate businessId.
		3. Call the getNotificationsByBusinessId method in the business service.
			1. Get the local business using business id.
			2. Get User from the local business.
			3. Check if the business user has the permission to view the business.
			4. Create the pageable request and call the getNotificationsByBusinessUser method in the Notifications Service.
				1. Call the findByUserAndVisible method from the notification repository to get the notifications.
			5. Call the getUnreadNotificationCount method from the Notification Service to get the count.
			6. Send the Business Notifications Response.

 6.  Mark as read API:
 
			 URI: /biz/businesses/58790731e4b0c0742c517a98/notifications/mark-as-read
			 Role : ROLE_BUSINESS_ADMIN
			 Request Body: List of notificationIds
			 1. Check if user is authenticated.
			 2. Validate business id and notfication Ids.
			 3. Call markAsReadNotification method from business service. 
				 1. Get the local business from the businessId.
				 2. Check if local business is not null.
				 3. Check if user has access to update the business settings.
				 4. Call the markAsReadNotification method of the Notification Service.
					 1. Get the notifications of all the notification Ids using Notification Repository.
					 2. Check all the notifications are present and it is not empty.
					 3. Check if it is valid notification ids:
					`notification.getUser().equals(localBusiness.getUser()) &&`  `LOCAL_BUSINESS.equals(notification.getUser().getUserType().name())`
					4. Set the checked flag to true for all the notifications whose flag is false.

7. Mark all as read API:
8.  Performance Summary:
		URI: /biz/businesses/58790731e4b0c0742c517a98/performance-summary
		 Role: ROLE_BUSINESS_ADMIN , ROLE_BUSINESS_VIEWER
		 1. Validate business id.
		 2. Check if user is authenticated.
		 3. Call the fetchAnalyticsData method of the business service.
			 1. Get the local business using business id.
			 2. Check the permission if  user can view the business.
			 3. Get Analytics Count and check if it greater than BIZ_VIEW_COUNT_THRESHOLD(20) if true then fetch last six months analytics data and return the last of Analytics Metrics.
		4. Send the Performance Summary Response.

9.  Performance Details:
		URI: /biz/businesses/58790731e4b0c0742c517a98/performance-details.
		 Role: ROLE_BUSINESS_ADMIN , ROLE_BUSINESS_VIEWER
		 1. Validate start date and end date.
		 2. Check if user is authenticated.
		 3. Call the fetchPerformanceDetails method of business Analytics Service.
			 1. Get the local business by business id.
			 2. Check if the user has access to view the business. 
			 3. Build business Performance Analytics Detail init values.
			 4. Check if it is eligible for hourly, monthly and daily period and get business performance analytics details group by selected period. 
			 5. Return business performance analytics detail response.
		4. Convert domain to response and send back the response.

10.  Get Categories API:
		URI: /biz/businesses/58790731e4b0c0742c517a98/categories.
		Role: ROLE_BUSINESS_ACCOUNT_ADMIN , ROLE_BUSINESS_ADMIN , ROLE_BUSINESS_VIEWER
		1. Check if user is authenticated.
		2. Validate business id.
		3. Call the getCategories method in the  business service.
			1. Retrieve the local business using the business id.
			2. Check if local business is null.
			3. Check if the user has permission to view the business.
			4. Get the list of macro categories.
				1. Category ----> Macro category.
				2. Category mother ----> Master category.
				3. Primary Flag to check if the category is primary or not.
			5. Sort the categories by  macro category name.
			6. Set the primary = true if `category.getId().equals(localBusiness.getPrimaryCategoryId())`
			7. Send back the category response.
				1. List of Categories.
				2. Category Attributes.
				3. Metadata
		4. Sort the category attributes by field group name and return the category response.
	
11.  Delete Category API:
		URI: /biz/businesses/58790731e4b0c0742c517a98/category/{categoryId}
		Role: ROLE_BUSINESS_ACCOUNT_ADMIN , ROLE_BUSINESS_ADMIN
		 1. Validate business id and category id.
		 2. Check if user is authenticated.
		 3. Call the removeCategoryAndAttributes method in business service.
			 1. Get the local business by business id.
			 2. Get the category from the local business using categoryId in the request.
			 3. Check if business user has access to view the business and if user and business related to account.
			 4. You can't remove the last business category or primary category.
			 5. Remove the category from the local business and set the updated categories.
			
12.  Add Category to business API:
		URI: /biz/businesses/58790731e4b0c0742c517a98/category
		 Role: ROLE_BUSINESS_ADMIN , ROLE_BUSINESS_ACCOUNT_ADMIN
		 Request Body: 
				 {
	
	    "category_id":"   5878dcd3e4b01d34728689e8 ",
	
	    "category_fields": [
	
	        {
	
	            "field_id": "5878dcdae4b0f09f93382fa9",
	
	            "field_type": "text-box",
	
	            "value": "      this        is test\n \t \rimjancinaidcn i am on   new line\n    testing you    "
	
	        }
	
	    ]
	
	}
		 1. Validate business id and category id.
		 2. Check if user is authenticated.
		 3. Call the addCategoriesToBusiness method in business service.
			 1. Get local business with category fields using business id.
			 2. Check if business user can update the business.
			 3. Check if local business categories count >= MAX_MACRO_CATEGORIES_COUNT(20).
			 4. Validate category id.
			 5. Get Category by category id.
			 6. Validate if category type is not macro.
			 7. Check if local business already contains category.
			 8. Convert request category fields to map.
			 9. Convert local business category fields to map.
			 10. Validate request category field ids with local business category field id.
			 11. If category field is enabled and mandatory and request category field is null then throw exception.
			 12. Update default base field.
			 13. Check if given field type of a request is  matching with the actual category field type then throw exception else patch the default category field with request category field.
			 14. if request category field is null then check if business category fields mapping contains the categoryField field id, if yes then update the reference of default base field.
			 15. Add the updated default base field to the list.
			 16. Add category and  category fields and then save in the database while adding category fields replace it if they exist.


13.  Update Category Attribute API:
		
		URI: /biz/businesses/58790731e4b0c0742c517a98/category/attributes
		 Role: ROLE_BUSINESS_ADMIN, ROLE_BUSINESS_ACCOUNT_ADMIN
		 Request Body:
					`{`
	
	    `"category_fields": [`
	
	        `{`
	
	            `"field_id": "5878dcdae4b0f09f93382fa9",`
	
	            `"field_type": "text-box",`
	
	            `"value": "test \n tewst"`
	
	        `}`
	
	    `]`
	
	`}`
			
		1. Check if user is authenticated.
		2. Validate business id.
		3. Call the updateCategoryFieldsOfBusiness method in business service.
			1. Get local business with category fields using business id.
			2. Check if business user can update the business.
			3. Check if local business categories count >= MAX_MACRO_CATEGORIES_COUNT(20).
			4. Convert request category fields to map.
			5. Filter and set the local business category field in which base field is enabled.
			6. Validate request category field ids with local business category field id.
			7. If category field is enabled and mandatory and request category field is null then throw exception.
			8.  if updated category field is not null.
				1. if updatedCategoryfield field type is not equals to businessCategoryField field type, then throw the exception.
				2. Patch businessCategoryField with updatedCategoryField.
				3. Add businessCategoryField in the updatedCategoryField list.
			9. If updatedCategoryFields is not empty then update the business category fields.
				1. Remove the category field by field id.
				2. Add the new updated category fields.
				3. Save the local business entity and publish after save event.





canUpdateBusinessSettings  method - to check if business user is business admin then the business user has EDIT_ALLOWED permission.

	1. Get the business account role by business user id it returns BusinessUserToBusinessAccountRole object.
	2. Check if the role is equals to ROLE_BUSINESS_ACCOUNT_ADMIN or ROLE_BUSINESS_ACCOUNT_VIEWER.
		1. if the above condition is true, then get the business user to business roles using business user id and business id.
		2. Check if it contains any business level edit role = ROLE_BUSINESS_ADMIN.



	isUserHasBusinessAccountAdminPermission method - to check if the business user has business account admin role then assign USER_INVITE_ALLOWED permission.

	1. Call the isAccountAdmin method by passing business userid , business account id and ROLE_BUSINESS_ACCOUNT_ADMIN role.

	
			
				
			
	
		 
		
		
		
					 
			 
			 
		 
		
		
				
		
		
	