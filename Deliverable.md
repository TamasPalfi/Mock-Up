# Low Level Design

## Database and Libraries

Our database of choice is PostgreSQL. We are using the psycopg library as an adapter to make PostgreSQL work with our Django project. 

## Overall Description

Our design is abiding by the principles of having a tightly coupled set of objects. This minimizes our use of object inheritance.

Similarly, it allows our objects to establish a one to one connection with their associated table in the database, allowing for single instantiations of objects to communicate directly with the database. 

The database adapter, namely models.py in the case of our Django project, provides factory methods to introduce functionality such as *getting*, *setting*, *deleting*, and *creating* system objects. These are private methods that allow our team to create objects with private constructors and return them for use within the rest of the system. This allows other teams within our track to use these objects for whatever purposes needed. We also have a public API with *get*, *set*, *add*, and *remove* methods for each system object that are visible to the other teams for their own uses. In this case, the database knows of the various objects within the system.
      
## Public API

### Member API

| Return Type | Method Call                                                                                                                |
|-------------|----------------------------------------------------------------------------------------------------------------------------|
| Member      | `add_member(self, user_id, first_name, last_name, email, pw, cc_num, invited_by, user_type, birthday, address, phone number, date_created)`                                                                                                                              |           
| Member      | `get_member(self, obj_id)`                                                                                                   |
| Member     | `set_visibility(self, visibility: bool)`                                                                                     | 
| Member     | `set_email(self, new_email: string)`                                                                                    | 
| Member     | `set_password(self, new_password: str)`                                                                          | 
| Member     | `set_first_name(self, first_name: str)`                                                                          | 
| Member     | `set_last_name(self, last_name: str)`                                                                          | 
| Member     | `set_points(self, points: int)`                                                                          | 
| Member     | `set_user_type(self, type: str)`                                                                          | 
| Member     | `set_is_verified(self, verification: bool)`                                                                          | 
| Member     | `set_birthday(self, birthday: str)`                                                                          | 
| Member     | `set_address(self, address: str)`                                                                          | 
| Boolean     | `remove_member(self)`  
      
### Post API

| Return Type | Method Call                                                          |
|-------------|----------------------------------------------------------------------|
| Post        | `add_post(self, post_information: dict())`                             |
| Post     | `get_post(self)`                                            |
| Post     | `set_url(self, new_urls: list())`                      |
| Post     | `set_is_flagged(self, flag: bool)`                      |
| Post     | `set_content(self, new_content: str)`                      |
| Post     | `set_by_admin(self, by_admin: bool)`                      |
| Boolean     | `remove_post(self)`                                            |

### Comment API

| Return Type | Method Call                                        |
|-------------|----------------------------------------------------|
| Comment     | `add_comment(self, comment_information: dict())`     |
| Comment     | `get_comment(self)`                          |
| Comment     | `set_content(self, new_content: str)` |
| Comment     | `set_comment(self, by_admin: bool)` |
| Boolean     | `remove_comment(self)`                       | 

### Image API

| Return Type | Method Call                                                          |
|-------------|----------------------------------------------------------------------|
| Image       | `add_image(self, image_information: dict())`                           |
| Image       | `get_image(self)`                                              |
| Image       | `set_is_flagged(self, flag: bool)`                     |
| Image       | `set_by_admin(self, by_admin: bool)`                     |
| Boolean     | `remove_image(self)`                                           |

### Filter API

| Return Type | Method Call                                                          |
|-------------|----------------------------------------------------------------------|
| Boolean     | `add_filter(self, filter_information: dict())`                         |
| Filter      | `get_filter(self)`                                             |
| Boolean     | `set_filter_name(self, filter_name: str)`                    |
| Boolean     | `remove_filter(self)`                                          |

### Credit Card API

| Return Type | Method Call                                                                  |
|-------------|------------------------------------------------------------------------------|
| CreditCard  | `add_cc(self, params)`         |
| CreditCard  | `get_cc(self)`                                                      |
| CreditCard  | `set_holder(self, content: fk)` |
| CreditCard  | `set_expiration(self, expiration: str)` |
| CreditCard  | `set_in_use(self, in_use: bool)` |
| CreditCard  | `set_address(self, address: str)` |
| CreditCard  | `set_zipcode(self, zipcode: str)` |
| Boolean     | `remove_object_type(obj_id)`                                                   |

## Private API

We currently have private constructors that can only be accessed through private method calls in our DBAdaptor class which instantiate instances of:
   - `Member` objects
   - `Post` objects
   - `Image` objects
   - `Comment` objects
   - `CreditCard` objects
   - `Filter` objects
   
### Member

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Member Object     | `Member(self, user_id, first_name, last_name, email, pw, cc_num, post_id, points, visibility, invited_by, user_type, login_time, logout_time, date_created, birthday, address, phone_number)` |
                
### Comment 

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Comment Object    | `Comment(self, replies, post_id, user_id, content, date_created, by_admin)` |

### Post

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Post Object       | `Post(self, comments, image_id, user_id, urls, shortened_urls, date_created, date_modified, is_flagged, points_given, content, by_admin)` |

### Image

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Image Object      | `Image(self, filter_id, original_image_id, is_flagged, by_admin)` |

### Filter

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Filter Object     | `Filter(self, filter_id, preview_url)` |

### Credit Card

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Credit Card Object| `CreditCard(self, card_num, cvv, holder_name, exp_date, date_added, currently_used, user_id)` |


## Class Diagram

![alt-text](https://github.com/320-group4/High-Level-Requirements/blob/master/er1.png)

## Testing Plans

### Equivalence Testing/Partitioning

After we have implemented our unit testings for each unit method, we decided to proceed towards an equivalence testing suite. As a recap, the goal of equivalence testings is to split up the input domain ranges of each method into equivalent partitions, so as to only test on one possible object in that partition.  The idea is that by ensuring tests on one object in that range the method calls will work for all possible obejcts in that domain space.  There is one fatal problem with this testing method which is that just because two objects are supposed to be in the same class and act the same, doesn't necessarily mean that they will.  Thus, equivalence testing could miss those cases. Equivalence testing is typically performed on inputs due to having a larger domain of values, and not typically performed on outputs due to methods usually returning one type.  This will also help us pick out edge cases for we will have effectively mapped out all the domain ranges.  

For our database implementation, each user is very unique for every person has their own, separate data.  For example, the values of possible ranges for attribute fields for The Member object such as name, email, date created, etc. can effectively be whatever the user or person wants (with the exception to some edge cases detailed earlier).  There isn't really much room to set up different equivalence classes/partitions since the users are all pretty similar but with different data.  The only case we thought of that would be partition into different equivalence classes is Member's user_type attribute which can be Member, Idol, or Admin.  However, the testing approach for those differing fields would not be different. Thus, the way we have decided to approach the equivalence class testing for the database is to just basically give one equivalence class for each object such as Member, Post, Comment, Etc.  By testing on say two or three instances of those classes, we can show that the methods for add, get, set, and remove work for all objects in that equivalence class or basically the whole object.

### Integration Testing

Our current integration testing is working with the web server teams to establish a reliable connection that allows us to establish a relationship with the model teams. 

A successful passing of this integration testing would result in model teams being able to successfully add and remove items from the database, where the items they add are not from our predefined unit tests.

Instead, we would be testing that there is a route of communication from the model teams, through the web server, then to our database. 

Similarly, we would test that there would be a connection that would allow the model teams to send a get call that would go to the web server, communicate with our database, and we would then return the specified item upon validation that it exists. 

### Unit Tests

Helper Functions: As is the case with the other object classes, we need to be able to create instances of each object and associated      objects following it in the hierarchy for testing purposes. We will use the following helper functions throughout our tests: 

   - `Create Member`: 
      - Preconditions/Parameters: `name, invitedBy` (optional)
      - Postconditions: Returns a `Member` object with the following parameters set: `visibility, invited_by, email, password, username, points, user_type, is_verified, birthday, address`
   - `Create Post`: 
      - Preconditions: `content, user` (optional)
      - Postconditions: Returns a `Post` object with the following parameters set: `user, url, is_flagged,content, by_admin`
   - `Create Comment`:
      - Preconditions: `content, user, post, replies` (optional) 
      - Postconditions: Returns a `Comment` object with the following parameters set: `user, post, replies, content, by_admin`  
   - `Create Credit Card`: 
      - Preconditions: `user
      - Postconditions: Returns a `CreditCard` object with the following parameters set: `user, card_num, cvv, holder_name, card_expiration, currently_used, address, zipcode` 
   - `Create Image`:
      - Preconditions: `user, post, image`
      - Postconditions: Returns a `Image` object with the following parameters set: `user, post, current_image, is_flagged, by_admin`
   - `Create Filter`:
      - Pre-Conditions: `image`
      - Post-Conditions: Returns a `Filter` object with the following parameters set: `image, filter_name`

#### Member Test Cases

***Test Case #1: Creating a Member***

`test_create()`

Description: This case tests that the creation of a new `Member` works as intended.

Steps:

1. Create a new `Member` object. 
2. Verify that new `Member` created is an instance of `Member`. 
3. Verify that total count of Members is 1 - ensuring that new `Member` has been added.
Note: When the `Member` object was instantiated, it was set to a `Member` object - but this could just as easily have been `Admin` or `Idol`, as these do the same function as Member. Testing for `Member` shows it will work for `Idol` and `Admin`. 

***Test Case #2: Creating a Member***

`test_class_create()`

Description: Similar to ***Test Case #1***, except this case tests for a second `Member` object being added to Member table in database.

Steps:

1. Create a new `Member` object. 
2. Verify that new `Member` created is an instance of Member. 
3. Verify that total count of Members is 2 - ensuring that new `Member` has been added.
Note: When the `Member` object was instantiated, it was set to a `Member` object - but this could just as easily have been `Admin` or `Idol`, as these do the same function as Member. Testing for `Member` shows it will work for `Idol` and `Admin`. 

***Test Case #3: Retrieving a Member Attribute***

`test_get_specific_data()`

Description: This case tests for retrieval of data from within the database. 

Steps:
1. Create a new `Member` object with a `name` attribute "user_name". 
2. Call for the username of that variable created using the `<member>.data['username']` to retrieve the username.
3. Compare this username to the username used upon creation of `Member` object. Confirm these values match.
      
***Test Case #4: Retrieving a Member Attribute By ID***
   
`test_get_byid()`

Description: This case tests that a `Member` attribute can be retrieved through use of `Member’s id`. 

Steps: 
1. Create a new `Member` object with a name attribute "new_user". 
2. Get `id` of `Member` using `<member>.data['id']` call. 
3. Call for `Member` object using `Member.objects.get(id=new_member_id).data()['username']` call. 
4. Verify that the `Member name` is equal to "new_user". 
      
***Test Case #5: Editing a Member***
   
`test_edit()`

Description: This case Tests that after editing a `Member` object, the change is seen in the database. 

Steps:
1. Create a new `Member` object with a name attribute "new_user".
2. Change `Member’s` username by calling  `<member>.set_username("DIFFERENT_USER")`.
3. Call the username of this `Member` using the call `<member>.data['username']`.
4. Test that username is equal to `"DIFFERENT_USER"`. 
   
***Test Case #6: Setting Member Points***

`test_set_points()`

Description: This case tests that changing a `Member’s` point value shows the change in the database. 

Steps: 

1. Create a new `Member` without defining the points field. 
2. Take this `Member` and call the method `set_points()`, changing member points to any integer value - for this test, we will set the `Member’s` points to 50. 
3. Retrieve `Member` points, checking that it matches the integer value of 50. 

***Test Case #7: Deleting Member***

`test_delete()`

Description: This case tests that a `Member` is successfully deleted from the database. 

Steps:
1. Create a new `Member`, confirming that it exists within the database by calling `Member.objects.count()`, checking for a return value of 1. 
2. Retrieve the id of the newly created `Member` object.
3. Delete this `Member` by calling `Member.objects.filter(id=id).delete()`.
4. Call `Member.objects.count()` again, confirming that the value is 0 - showing successful deletion.

***Test Case #8: Remove Method for Member***

`test_remove_method()`

Description: This cas tests remove method is working correctly. `remove_member()` is defined in Models.py.

Steps: 
1. Create a new `Member`, confirming that it exists within the database.
2. Call on `remove_member()`, confirming the count of Members is equal to 0. 

***Test Case #9: Retrieving Member Attribute Invitedby***

`test_inviteby()`

Description: This case tests for proper retrieval of the user in which a `Member` was invited by. 

Steps:
1. Create 2 `Members`, one is an `Idol` and one is a `Member`. The `Idol` invited the `Member` in this case. 
2. Retrieve the user that invited the `Member` , extracting their username. 
3. Test to see that the username is equal to the name of the `Idol`.

#### Post Test Cases

***Test Case #1: Creating a Post***

`test_create()`

Description: This case tests that the creation of a new `Post` works as intended.

Steps:

1. Create a new `Member` object and a new `Post` object. 
2. Verify that the created `Post` object is an instance of `Post`. 
3. Verify that the `Post` object has successfully been added to the `Post` table by checking the count (given the empty database, after creation it will equal 1). 

***Test Case #2: Author of Post***

`test_writer()`

Description: This case tests that the correct author is set for a `Post` object. 

Steps: 
1. Create a new `Member` object and new `Post` with the `Member` as the author. 
2. Extract the `user_id` from the `Post`. 
3. Use the user_id to extract the username from the `Member` object, testing that it matches the name of the `Member` created in step 1. 

***Test Case #3: Change of URL***

`test_set_urls()`

Description: This case tests that the URL of a post is successfully changed.

Steps: 
1. Create a new `Member` object and a new `Post` object.
2. Define the url of the `Post` to be www.cooltest.com using the `set_urls` function defined in Models.py. 
3. Extract the url name from the `Post`, and test against the name www.cooltest.com

Note: This shows a change for other fields of `Post` will have similar responses.

#### Comment Test Cases

***Test Case #1: Creating a Comment***

`test_create()`

Description: This case tests that the creation of a new `Comment` works as intended.

Steps: 
1. Create a new `Member` object and a new `Comment` object. 
2. Verify that the created `Comment` object is an instance of `Comment`. 
3. Verify that the `Comment` object has successfully been added to the `Comment` table by checking the count (given the empty database, after creation it will equal 1).

***Test Case #2: Checking Content of a Comment***

Description: This case tests that the created `Comment` object contains the correct `content` attribute.

Steps:
1. Create a new `Member` object and a new `Comment` object with the `content` attribute "Hello World!". 
2. 



#### Credit Card Test Cases

***Test Case #1: Creating a Credit Card***

`test_create()`

Description: This case tests that the creation of a new *Credit Card* works as intended.

Steps: 
1. Create a new `Member` object and a new `CreditCard` object. 
2. Verify that the created `CreditCard` object is an instance of `CreditCard`. 
3. Verify that the `CreditCard` object has successfully been added to the `CreditCard` table by checking the count (given empty database, after creation it will equal 1). 

#### Image Test Cases

***Test Case #1: Creating an Image***

`test_create()`

Description: This case tests that the creation of an `Image` works as intended.

Steps:
1. Create a new `Member` object and a new `Post` object. 
2. Use the created objects from #1 as parameters in order to create a new `Image` object. 
3. Verify that the `Image` object has successfully been added to the `Image` table by checking the count (given empty database, after creation it will equal 1).
4. Test that the `Image` object is the same instance as the `Image` built through the API's `create` call. 

Note: There is another image variable that is a temporary photo file object created in order to test for comparisons.

***Test Case #2: Creating an Image***

Description: This case tests that the creation of an 'Image' object works by adding a new 'Image' to the database. Same as *Test Case #1* but ensures that the API `create` call successfully adds data to the database.

Steps:
1. Follow Steps 1 - 2 from *Test Case #1*. 
2. Verify that the `Image` object has successfully been added to the `Image` table by checking the count - which should now equal 2. 
3. Test that the `Image` object is the same instance as the `Image` built through the API's `create` call. 

***Test Case #3: Retrieving Image Attribute***

Description: This case tests that a `Image` attribute can be retrieved. 

Steps: 
1. Create a new `Member` object and `Post` object using .
2. Create a new `Image` object using the above as parameters.
3. Retrieve the `Image` `id`. 
4. Verify that the original_image `id` matches the `id` retrieved from the `get` call. 

***Test Case# 4: Setting Image Attribute***

Description: This case tests that a change in an `Image's` attribute shows in the database.

Steps: 
1. Create a new `Member` object and `Post` object using .
2. Create a new `Image` object using the above as parameters.
3. Set a new `Image` `id`.  
4. Verify that the `id` value retrieved from the database is equivalent to the value that was set. 

***Test Case# 5: Deleting Image Attribute***

Description: This case tests that an `Image` is successfully removed from the database.

Steps:
1. Create a new `Image` object. 
2. Verify it's existence in the database with an assert statement, check that count = 1. 
2. Delete the `Image` object using it's `id`. 
3. Verify that the database is empty, count = 0. 
 
#### Filter Test Cases

***Test Case #1: Creating a Filter Object***

Description: This case tests that the creation of a `Filter` object works as intended.

Steps: 
1. Create a `Member`, `Post` and `Image` object.
2. Using the above objects as parameters, create a `Filter` object. 
3. Verify that the object count is equal to 1 (given an empty database). Ensure no duplicate. 
4. Verify that the `Filter` is the same instance/object as the `Filter` build through the API's `create` call. 
      
 ***Test Case #2: Setting a Filter Attribute***
 
 Description: This case tests that the updating of a `Filter` object works as intended.
 
 Steps: 
1. Create a `Member`, `Post` and `Image` object.
2. Using the above objects as parameters, create a `Filter` object.
3. Set a new `Filter` `id`. 
3. Test that the updated `Filter` `id` is equivalent to the `id` retrieved from the database. 

### Important Cases and Edge Cases

   1. Ensure that data critical to a user's verification is stored correctly in the database
      - Credit card number is 16 digits
      - Ensure 'cvv' is either 3 or 4 digits
      - Expiration date must be valid (and in the future)
      - Birth month is between numeric values 1 - 12
      - Birth day is between numeric values 1 - 30
      - Ensure user type is either `Member`, `Admin`, or `Idol`
   
   2. Ensure retrieving data from database table returns expected value
      - 2.a. Ensure ID is valid
      - 2.b. Ensure the column retrieving from is in database table
      - 2.c. Ensure the column we're retrieving from is filled and contains data
   
   3. Ensure updating a table updates the right thing
      - 3.a. Ensure column exists
      - 3.b. Ensure id is valid
      - 3.c. Ensure all data is valid (see #1)
   
   4. Ensure deleting from a table entry removes from database
      - 4.a. Ensure id is valid
