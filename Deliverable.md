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
| Member      | add_member(self, user_id, first_name, last_name, email, pw, cc_num, invited_by, user_type, birthday, address, phone number, date_created)                                                                                                                              |           
| Member      | get_member(self, obj_id)                                                                                                   |
| Member     | set_visibility(self, visibility: bool)                                                                                     | 
| Member     | set_email(self, new_email: string)                                                                                    | 
| Member     | set_password(self, new_password: str)                                                                          | 
| Member     | set_first_name(self, first_name: str)                                                                          | 
| Member     | set_last_name(self, last_name: str)                                                                          | 
| Member     | set_points(self, points: int)                                                                          | 
| Member     | set_user_type(self, type: str)                                                                          | 
| Member     | set_is_verified(self, verification: bool)                                                                          | 
| Member     | set_birthday(self, birthday: str)                                                                          | 
| Member     | set_address(self, address: str)                                                                          | 
| Boolean     | remove_member(self)  
      
### Post API

| Return Type | Method Call                                                          |
|-------------|----------------------------------------------------------------------|
| Post        | add_post(self, post_information: dict())                             |
| Post     | get_post(self)                                            |
| Post     | set_url(self, new_urls: list())                      |
| Post     | set_is_flagged(self, flag: bool)                      |
| Post     | set_content(self, new_content: str)                      |
| Post     | set_by_admin(self, by_admin: bool)                      |
| Boolean     | remove_post(self)                                            |

### Comment API

| Return Type | Method Call                                        |
|-------------|----------------------------------------------------|
| Comment     | add_comment(self, comment_information: dict())     |
| Comment     | get_comment(self)                          |
| Comment     | set_content(self, new_content: str) |
| Comment     | set_comment(self, by_admin: bool) |
| Boolean     | remove_comment(self)                       | 

### Image API

| Return Type | Method Call                                                          |
|-------------|----------------------------------------------------------------------|
| Image       | add_image(self, image_information: dict())                           |
| Image       | get_image(self)                                              |
| Image       | set_is_flagged(self, flag: bool)                     |
| Image       | set_by_admin(self, by_admin: bool)                     |
| Boolean     | remove_image(self)                                           |

### Filter API

| Return Type | Method Call                                                          |
|-------------|----------------------------------------------------------------------|
| Boolean     | add_filter(self, filter_information: dict())                         |
| Filter      | get_filter(self)                                             |
| Boolean     | set_filter_name(self, filter_name: str)                    |
| Boolean     | remove_filter(self)                                          |

### Credit Card API

| Return Type | Method Call                                                                  |
|-------------|------------------------------------------------------------------------------|
| CreditCard  | add_cc(self, params)         |
| CreditCard  | get_cc(self)                                                      |
| CreditCard  | set_holder(self, content: fk) |
| CreditCard  | set_expiration(self, expiration: str) |
| CreditCard  | set_in_use(self, in_use: bool) |
| CreditCard  | set_address(self, address: str) |
| CreditCard  | set_zipcode(self, zipcode: str) |
| Boolean     | remove_object_type(obj_id)                                                   |

## Private API

We currently have private constructors that can only be accessed through private method calls in our DBAdaptor class which instantiate instances of:
   - Member objects
   - Post objects
   - Image objects
   - Comment objects
   - Credit Card objects
   - Filter objects
   
### Member

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Member Object     | Member(self, user_id, first_name, last_name, email, pw, cc_num, post_id, points, visibility, invited_by, user_type, login_time, logout_time, date_created, birthday, address, phone_number) |
                
### Comment 

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Comment Object    | Comment(self, replies, post_id, user_id, content, date_created, by_admin) |

### Post

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Post Object       | Post(self, comments, image_id, user_id, urls, shortened_urls, date_created, date_modified, is_flagged, points_given, content, by_admin) |

### Image

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Image Object      | Image(self, filter_id, original_image_id, is_flagged, by_admin) |

### Filter

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Filter Object     | Filter(self, filter_id, preview_url) |

### Credit Card

| Return Type       | Method Call                |
|-------------      |----------------------------|
| Credit Card Object| CreditCard(self, card_num, cvv, holder_name, exp_date, date_added, currently_used, user_id) |


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

#### Member Test Cases

***Test Case #1: Creating an Member***

*test_create()*

Description: This case tests that the creation of a new *Member* works as intended.

Steps:

1. Create a new *Member* object. 
2. Check that new *Member* created is an instance of Member. 
3. Check that total count of Members is 1 - ensuring that new *Member* has been added.
Note: When the *Member* object was instantiated, it was set to a *Member* object - but this could just as easily have been *Admin* or *Idol*, as these do the same function as Member. Testing for *Member* shows it will work for *Idol* and *Admin*. 

***Test Case #2: Creating an Member***

*test_class_create()*

Description: Similar to ***Test Case #1***, except testing for a second *Member* object being added to Member table in database.

Steps:

1. Create a new *Member* object. 
2. Check that new *Member* created is an instance of Member. 
3. Check that total count of Members is 2 - ensuring that new *Member* has been added.
Note: When the *Member* object was instantiated, it was set to a *Member* object - but this could just as easily have been *Admin* or *Idol*, as these do the same function as Member. Testing for *Member* shows it will work for *Idol* and *Admin*. 

***Test Case #3: Retrieving a Member Attribute***

*test_get_specific_data()*

Description: Testing to retrieve data from within the database. 

Steps:
1. Create a new *Member* object with a name attribute "user_name". 
2. Call for the username of that variable created using the <member>.data['username'] to retrieve the username.
3. Compare this username to the username used upon creation of *Member* object. Confirm these values match.
      
***Test Case #4: Retrieving a Member Attribute By ID***
   
*test_get_byid()*

Description:

Steps: 


This test is designed to test that we can retrieve data from the tables using the ID of each row.  We first start by creating a new Member with the user name "new-user".  We then get the ID if this member by using the same <member>.data['id'] call we made earlier, but this same asking for the "id" field.  We then make a call asking for the whole Member object by making the call Member.objects.get(id=new_member_id).data()['username'], passing in the id we just retrieved into the id field to that the get call can return back the whole Member object.  With this Member object, we can use <member>.data()['username'] again, but only asking for the user name.  Lastly we check to see if this user name is equal to the name "new-user" which we used at the beginning to set the name of the new Member.
      
***Test Case #4: Editing a Member***
   
*test_edit()*

Description:

Steps: 

This test is testing if we can make an edit to the Member object.  We first start by creating a new Member object, and setting its user name to "new-user".  We then change its user name by calling the call <member>.set_username("USER") but pass in a different name to what we oroginially made it to be.  After this we have to call the user name of this member from the Member table by using the same call <member>.data['username'].  We then test to see if this username is equal to "USER".
   
***Test Case #5: Setting Member Points***

*test_set_points()*

Description:

Steps: 

This test is to show that we can change the point value of each member quickly and easily.  We start off by creating a new Member, but leaving out the points field and not defining it in the Member creation.  We then take this member and call the method set_points(), and set the amount of points to whetever, we can set it to 1.  When we retrieve the amount of points that the member has, and check to see that it equals 1.

***Test Case #6: Deleting Member***

*test_delete()*

Description:

Steps:

Here we are testing to see if we can create and delete a Member successfully.  We start by creating a new member, then checking to see if it is in the table by calling Member.objects.count() and seeing if that returns a 1.  We then retrieve the id of that object in the table, and delete this by calling Member.objects.filter(id=id).delete(), this will delete the object in the table of that id.  Lastly we call the Member count again and check to see if it is now back at 0, to show that this Member has successfully been deleted.

***Test Case #7: Remove Method for Member***

*test_remove_method()*

Description:

Steps: 

This test is testing to see if our remove method is working properly.  This remove_member() methond is defined in our Models.py.  We start off by first creating a member, then once this is complete we remove the member using the function remove_member().  We then test to see if the count of the Members table is equal to 0.

***Test Case #8: Retrieving Member Attribute Invitedby***

*test_inviteby()*

Description:

Steps:

In this test, we are testing to see if we can propery retireve the person who the member was invited by.  We start off by creating two members, one is an idol and the other is a member in which we add the idol as the person who the member was invited by.  We then retrieve the user that invited the member, and extract the user name from that user.  We then test to see  if this user name is equal to the user name of the idol.

#### Post Test Cases

***Test Case #1: ***

*test_create()*

We start this test by creating a new Member object as well as a new Post object.  We first test to see if this post object is an instance of a Post.  Lastly we test to see if there is a Post object in the Post table, showing that a post has been successfully added to the post table.  It is important to note that since we were able to successfully create the post with information added to the user field, we do not have to test that all fields work because we now know they work.

***Test Case #2:***

*test_writer()*
The point to this test is to test to make sure that the writer of the post is the writer we set it to.  We start here by first creating a new Member and a new Post with the Member as the author.  Then we excract the user_id from the post, to find out who the user was who wrote it.  We then use this id to extract the username from the Member object, and test this user name agaist the name we used to create the Member.  

***Test Case #3:***

test_set_urls()
This test is testing that we can change the url of the post successfully.  Being able to do this proves that we can also change every other field in the Post object successfully, because when changing anything in the Post object we just perform the same operation.  So doing it once would prove that we can do it for all of them.  We first start off by creating a Member object and a Post object.  We then define the url of the post to be www.cooltest.com using our set_urls function defined in Models.py.  After this we extract the url name from the post, and test this against the name www.cooltest.com.

#### Comment Test Cases

#### Credit Card Test Cases

#### Image Test Cases

Helper Functions: As is the case with the other object classes, we need to be able to create instances of each object and associated      objects following it in the hierarchy for testing purposes. For the Image test cases, we create the following helper functions:
   - Create Member: 
      - Pre-Conditions/Params: Name, invitedBy (optional param)
      - Post-Conditions: Returns a Member object with the following params set: visibility, invited_by, email, password, username,                                points, user_type, is_verified, birthday, and address.
   - Create Post: 
      - Pre-Conditions: Content, User (optional)
      - Post-Conditions: Returns a Post object with the following params set: user, url, is_flagged,content, and by_admin.
   - Create Image:
      - Pre-Conditions: User, post, image.
      - Post-Conditions: Returns a Image object with the following params set: user, post, current_image, is_flagged, and by_admin.

***Test Case #1: Creating an Image***

Description: This case tests that the creation of an image works as intended.

Steps:

1. Create a *member* object and a *post* object. 
2. Use the created objects from #1 as parameters in order to create a new *image* object. Note: There is another another image variable that is a temporary photo file object created in order to test for comparisons.

      Assert Statements: 
      - Test that the image variable is the same instance/object as that image built through the API's create call.  
      - Test that the object count is equal to one, ensuring no duplicate creation. 
      
***Test Case #2: Creating an Image***

Description: 

Steps:

Case 2: Creating Image Test # 2
   This case tests that the creation of an image actually works by adding the new image to the database. It is essentially the same as      test case #1, but is ensuring that our API create call adds data to the database
   
   Assert Statements:
      - Test that the image variable is the same instance/object as that image built through our API's create call.  
      - Test that the object count is equal to two.  Basically, to ensure our first test case actually added to the database.  

***Test Case #3: Retrieving Image Attribute***

Description:

Steps:

Case 3: Getting Image attribute
  This case tests that the API get call works as intended to retrieve the Image data for a specific attribute.  We will test that the     original_image_id matches to what is in the database.  It works by first creating the respective Member and Post Objects with that the   Image object uses as parameters for creation.  We then get the original_image_id and do an assert statement to ensure they are equal. 
  
  Assert Statements:
      - Test that the original_image_id recieved from GET call is actually the ID specified.  

***Test Case# 4: Setting Image Attribute***

Description:

Case 4: Setting Image attribute
   This case tests that the API set call works as intended to update the associated attribute field for the Image object to be changed.    It works by first creating the respective Member and Post Objects with that the Image object uses as parameters for creation.  Then,    a new image ID is set and this is compared to the associated original_image_id that is retrieved from the database.
   
   Assert Statements:
      -Test that the updated original_image_id was reflected in the database and is equivalent. 

***Test Case# 5: Deleting Image Attribute***

Description:

Case 5: Deleting Image attribute
   This case tests that the API delete call works as intended to remove the associated Image is removed from the database. After            creation of the image, we do an assert statement to see that the image is in the database.  We then delete the image with the use of    it's orignal_image_id.  Finally, we check to see that the database is empty as it should be
   
   Assert Statements:
      -Test that the number of Image objects in database is 1 after creation.
 
#### Filter Test Cases
Helper Functions: As is the case with the other object classes, we need to be able to create instances of each object and associated    objects following it in the hierarchy for testing purposes. For the Image test cases, we create the following helper functions:
   - Create Member: 
      - Pre-Conditions/Params: Name, invitedBy (optional param)
      - Post-Conditions: Returns a Member object with the following params set: visibility, invited_by, email, password, username,                                points, user_type, is_verified, birthday, and address.
   - Create Post: 
      - Pre-Conditions: Content, User (optional)
      - Post-Conditions: Returns a Post object with the following params set: user, url, is_flagged,content, and by_admin.
   - Create Image:
      - Pre-Conditions: User, post, image.
      - Post-Conditions: Returns a Image object with the following params set: user, post, current_image, is_flagged, and by_admin.
   - Create Filter:
      - Pre-Conditions: Image
      - Post-Conditions: Returns a Filter object with the following params set: image, filter_name.

***Test Case #1: Creating a Filter Object***

Description:

Case 1: Creating a Filter Object
   This case tests that the creation of an Filter object works as intended. It works by creating a member, post, and image object first,    and using those created objects as parameters to make an Filter object.  There is another filter variable that is essentially a          temporary photo filter object created to test for comparisons.
   
   Assert Statements:
      - Test that the filter variable is the same instance/object as that filter built through our API's create call.  
      - Test that the object count is equal to one.  Basically, to ensure create didn't make a duplicate. 
      
 ***Test Case #2: Setting a Filter Attribute***
 
 Description:

 Case 2: Setting a Filter Object's Attribute
   This case test that the updating of a filter object works as intended.  We will be testing it by changing the filter_id attribute and    ensuring that it is changed in the database as well.
   
   Assert Statements:
      - Test that the updated filter_id was reflected in the database and is equivalent. 

### Important Cases and Edge Cases

   1. Ensure that data critical to a user's verification is stored correctly in the database
      - Credit card number is 16 digits
      - Ensure 'cvv' is either 3 or 4 digits
      - Expiration date must be valid (and in the future)
      - Birth month is between numeric values 1 - 12
      - Birth day is between numeric values 1 - 30
      - Ensure user type is either Member, Admin, or Idol
   
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
