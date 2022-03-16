# Assignment
## Test cases
1.Verify when launch the app Broken-hashserve, once it is installed succesfully, app waits for http connection

2.Verify app answer on the PORT specified in the PORT environment variable (PORT=8088)

3.Verify a POST to /hash should accept a password and return a job identifier immediately, then wait 5 seconds and compute the password hash.
Steps:
- Open the app;
- Click on Import tab in Postman;
- Choose Raw text;
- Put in the text field:   curl --location --request POST 'http://127.0.0.1:8088/hash' \
--header 'Content-Type: application/json' \
--data-raw '{"password":"angrymonkey"}'
Click Continue
Click Import
Click Send
Expected result: A job identifier returned immediately (in this case password ID '1')

Issue founded:
1.A job returned after 5 sec, from requirements it should be immediately, only for registering password hash it should take 5 sec, which is backend, and I can't see this in Postman.
2.Tried to changed in the body word password with a mistake, but it still succesfully creates password ID.
Concern: As I got ID as my job identifier I can't test the hashing algorithm SHA512, tried different online encoder/decoder, but solutions haven't found.

4.Verify a GET to /hash should accept a job identifier. It should return the base64 encoded password hash for the corresponding POST request.
Steps:
- App is launched
- Click on Import tab in Postman;
- Choose Raw text;
- Put in the text field: curl --location --request GET 'http://127.0.0.1:8088/hash/1' \
--header 'Content-Type: application/json' \
--data-raw ''
Click Continue
Click Import
Click Send
Expected result: Is accepted job edentifier and reurned base64 encoded password.

5.Verify a GET to /hash shouldn't accept haven't posted job identifier, for example 'http://127.0.0.1:8088/hash/2' (As previos POST job identifier was '1', so changed to '2' in a GET request should returned 404.
Steps: 
In the last request changed in the end of the url to '2' instead '1'
Click Send
Expected result: Job identified is Hash not found.
Issue: If job identified is Hash not found it should answered with 404 that means data not found, but we got 400 Bad request.

6.Verify A GET to /stats should accept no data. It should return a JSON data structure
for the total hash requests since the server started and the average time of a
hash request in milliseconds.
Steps:
- App is launched
- Click on Import tab in Postman;
- Choose Raw text;
- Put in the text field: curl --location --request GET 'http://127.0.0.1:8088/stats' \
--header 'Content-Type: application/json' \
--data-raw ''
Click Continue
Click Import
Click Send
Expected result: Accept no data. It returns a JSON data structure
for the total hash requests since the server started and the average time of a
hash request in milliseconds.
Issue: it returns only total POST requests. No count for GET.

7.Verify app support a graceful shutdown request, respond with a 200 and shutdown.
Steps:
- App is launched
- Click on Import tab in Postman;
- Choose Raw text;
- Put in the text field:curl --location --request POST 'http://127.0.0.1:8088/hash' \
--header 'Content-Type: application/json' \
--data-raw 'shutdown'
Expected result: respond with a 200 and shutdown

8.Verifiy after Shutdown no additional password requests should be allowed when shutdown is pending.
Steps:
- Go to the first POST request
- Click Send
Expected: could not send request

