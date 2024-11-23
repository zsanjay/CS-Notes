

1. Create Credit Score API:
	 URI : /biz/businesses/65b0bdd6dc19074fcdc4146e/aecb/credit-score
	 Role: ROLE_BUSINESS_ADMIN
	 Path Variable : businessId
	 Request Body : 
		{
		"business_name" : "Footwear shop"
		}

		1. Check if business user is authenticated or not.
		2. Get the local business by business Id.
		3. Check if business user can update the business setting.
		4. Convert request to domain.
		5. Call generateAECBCreditScore method of AecbCreditScoreService to get the score.
			1. Get all the AECB Credit score transactions of the particular business.
			2. Get the available Credit Score from all the transaction by calling getAvailableCreditScore method of AecbCreditScoreService.
			3. Check if credit score is already available then return the same.
			4. If success credit score is not available then check if transaction is in pending state (INITIATED or SCORE_AVAILABLE) then raise the exception saying transaction is in pending state.
			5. Call the generateAECBCreditScore method of AecbCreditScoreApiClient to generate the score using the AECB API.
				1. Generate and set the transaction Reference Id.
				2. Call the initiateAECBCreditScoreTransaction method of AecbCreditScoreService and save the transaction with start time and initiated state in the db.
				3. Call 3 API's of AECB to get the credit score.
					1. AECB authentication process.
					2. AECB score availability check process and updating the state to SCORE_AVAILABLE in the db.
					3. AECB get score process and save the credit score with the SUCCESS state in the db.
		6. Send the Aecb credit Score Transaction response.

	2.  Get Credit Score API:
		URI: /local-businesses/60979b1d18456c5ab7a3ba27/credit-score
		Role: ROLE_BUSINESS_ADMIN, ROLE_BUSINESS_VIEWER
		Path Variable : businessId

		1. Validate business id.
		2. Check if business user is authenticated or not.
		3. Get the local business by business Id.
		4. Check if business user can view the business.
		5. Call the getCreditScoreByBusinessId method of AecbCreditScoreService.
			1. Get all the AECB Credit score transactions of the particular business.
			2. Get available credit score transaction and return it if available.
			3. Get the latest transaction if score is not available.
			4. Check if the latest transaction has score unavailable then return the response with the failure reason else throw the exception with transaction is in failed state.
		6. Return the response.

	3.  Update Profile Visibility API:
		URI: /businesses/5878de34e4b0e1e4ea8cda01/aecb/credit-score/visibility
	    ROLE: ROLE_BUSINESS_ADMIN
		Path Variable : businessId
		Request body :
						`{`
			`"visibility" : "false"`
			`}`
	     