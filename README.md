# README.md

IMPORTANT: Once you've cloned this to your forked repository, ensure that you continuously update this document as you complete each task to demonstrate your ongoing progress.

Please include your shared repository link here: https://github.com/sagenabs/SeijiN-Assignment2

Example:
Choiru's shared repository: 


Make sure for **your case it is in Private**
## Access Database
1 **Plsql Cheat Sheet:**
You can refer to the PostgreSQL cheat sheet [here](https://www.postgresqltutorial.com/postgresql-cheat-sheet/).

2 **Know the Container ID:**
To find out the container ID, execute the following command:
   ```bash
   docker ps
    9958a3a534c9   testsystem-nginx           "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:80->80/tcp   testsystem-nginx-1
    53121618baa4   testsystem-frontend        "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   3000/tcp             testsystem-frontend-1
    c89e46ac94b0   testsystem-api             "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   5000/tcp             testsystem-api-1
    9f4aea7cf538   postgres:15.3-alpine3.18   "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   5432/tcp             testsystem-db-1
   ```
3. Running the application

**docker compose command:**
   ```bash
   docker compose up --build
   ```

4 **Access postgreSQL in the container:**
Once you have the container ID, you can execute the container using the following command:
You will see the example of running the PostgreSQL inside the container.
   ```bash
   docker exec -it testsystem-db-1 psql -U postgres
   choiruzain@MacMarichoy TestSystem % docker exec -it testsystem-db-1 psql -U postgres                                       
   psql (15.3)
   Type "help" for help.
   
   postgres=# \dt
             List of relations
    Schema |   Name   | Type  |  Owner   
   --------+----------+-------+----------
    public | contacts | table | postgres
    public | phones   | table | postgres
   (2 rows)
  
    postgres=# select * from contacts;
    id |  name  |         createdAt         |         updatedAt         
   ----+--------+---------------------------+---------------------------
     1 | Helmut | 2024-08-08 11:57:57.88+00 | 2024-08-08 11:57:57.88+00
    (1 row)
    postgres=# select * from phones;
    id | phone_type |   number    | contactId |         createdAt          |         updatedAt          
   ----+------------+-------------+-----------+----------------------------+----------------------------
     1 | Work       | 081431      |         1 | 2024-08-08 11:59:04.386+00 | 2024-08-08 11:59:04.386+00


postgres=# select * from contacts;
   ```
Replace `container_ID` with the actual ID of the container you want to execute.

## Executing API

### Contact API


1. Add contacts API  (POST)
```bash
http post http://localhost/api/contacts name="Choiru"
        
choiruzain@MacMarichoy-7 TestSystem % http post http://localhost/api/contacts name="Choiru"
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://localhost:3000
Connection: keep-alive
Content-Length: 102
Content-Type: application/json; charset=utf-8
Date: Thu, 08 Aug 2024 21:01:53 GMT
ETag: W/"66-FmPYAaIkyQoroDwP2JsAZjWTAxs"
Server: nginx/1.25.1
Vary: Origin
X-Powered-By: Express

{
"createdAt": "2024-08-08T21:01:53.017Z",
"id": 1,
"name": "Choiru",
"updatedAt": "2024-08-08T21:01:53.017Z"
}

```
2 Get contacts API  (GET)

```bash
http get http://localhost/api/contacts


choiruzain@MacMarichoy-7 TestSystem % http get http://localhost/api/contacts
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://localhost:3000
Connection: keep-alive
Content-Length: 104
Content-Type: application/json; charset=utf-8
Date: Thu, 08 Aug 2024 21:04:58 GMT
ETag: W/"68-V+4KuL2xahYt8YAkKG6rKdR7wHg"
Server: nginx/1.25.1
Vary: Origin
X-Powered-By: Express

[
{
"createdAt": "2024-08-08T21:01:53.017Z",
"id": 1,
"name": "Choiru",
"updatedAt": "2024-08-08T21:01:53.017Z"
}
]


```
3. Show/create the API commmand to delete the contacts (DELETE)

```bash
vboxuser@CSE5006:~/Desktop/SeijiN-Assignment2$ http delete http://localhost/api/contacts
HTTP/1.1 404 Not Found
Access-Control-Allow-Origin: http://localhost:3000
Connection: keep-alive
Content-Length: 154
Content-Security-Policy: default-src 'none'
Content-Type: text/html; charset=utf-8
Date: Thu, 21 Aug 2025 10:31:22 GMT
Server: nginx/1.25.1
Vary: Origin
X-Content-Type-Options: nosniff
X-Powered-By: Express

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Cannot DELETE /api/contacts</pre>
</body>
</html>





```

4. Show/create the API command to edit the contacts (PUT)
```
http get http://localhost/api/contacts/1/phones
HTTP/1.1 404 Not Found
Access-Control-Allow-Origin: http://localhost:3000
Connection: keep-alive
Content-Length: 160
Content-Security-Policy: default-src 'none'
Content-Type: text/html; charset=utf-8
Date: Thu, 21 Aug 2025 10:33:36 GMT
Server: nginx/1.25.1
Vary: Origin
X-Content-Type-Options: nosniff
X-Powered-By: Express

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Cannot PUT /api/contacts/1/phones</pre>
</body>
</html>

```
## Task 1 - USER INTERFACE CHANGES 
### 1.1
-Accessed frontend/src/components/Contact.js
- Line to update: <button className='button red' onClick={doDelete}>Delete </button>
- Updated to: <button className='button red' onClick={doDelete}>Delete Contact</button>
<img width="423" height="246" alt="image" src="https://github.com/user-attachments/assets/e656f09f-edd5-419c-a702-ace1716a236a" />

### 1.2
- Accessed frontend/src/components/NewPhone.js
- Line to update <button className='button green' type='submit'> Add </button>
- Updated Line to Dynamic name assignment: <button className='button green' type='submit'> Add {contact.name}'s Phone</button>
<img width="543" height="162" alt="image" src="https://github.com/user-attachments/assets/695bf0b2-7b99-40a8-9baf-d3aae62254c4" />


### 1.3
- Accessed frontend/src/components/NewPhone.js
- Line to replace: <input type='text' placeholder='Name' onChange={(e) => setName(e.target.value)} value={name}/>
- Replaced with:

  <select value={name} onChange={(e) => setName(e.target.value)}>
        <option value="Home">Home</option>
    <option value="Work">Work</option>
    <option value="Mobile">Mobile</option>
    <option value="Others">Others</option>
  </select>
<img width="435" height="169" alt="image" src="https://github.com/user-attachments/assets/a599afed-99d5-4e58-a78e-54be8cd5f8db" />

### 1.4
- Accessed frontend/src/components/PhoneList.js
- Line to replace: <th>Name</th>
- Replaced with: <th>Phone Type</th>
<img width="444" height="166" alt="image" src="https://github.com/user-attachments/assets/ae8894bf-5305-41e0-819d-0802a6807e33" />

## Task 2
### 2.1
<img width="775" height="387" alt="image" src="https://github.com/user-attachments/assets/ab3ee265-9ba2-4f75-9cd6-7caff497ce34" />

### 2.2
<img width="915" height="336" alt="image" src="https://github.com/user-attachments/assets/d22d5d7f-7fe1-4155-9771-ccac6266ee40" />

### 2.3
<img width="823" height="281" alt="image" src="https://github.com/user-attachments/assets/1f7c83a9-ae01-424d-a92d-68a8f5f3a580" />

### 2.4
<img width="950" height="277" alt="image" src="https://github.com/user-attachments/assets/1ee06282-d583-4197-aa6b-a0c6f657b8b8" />

### 2.5
<img width="791" height="338" alt="image" src="https://github.com/user-attachments/assets/272c0a79-c499-49bf-bd3b-5e27e1ce222b" />

### 2.6
<img width="1203" height="370" alt="image" src="https://github.com/user-attachments/assets/1606b26f-2cff-43ef-988b-eba9a9b0d7e6" />

### 2.7
<img width="905" height="281" alt="image" src="https://github.com/user-attachments/assets/579f9b65-cfe1-43fe-ace4-37a541f223e5" />

### 2.8
<img width="1228" height="299" alt="image" src="https://github.com/user-attachments/assets/c3bc5c26-2927-4ef2-9827-6248321c20f4" />

## Task 3
### 3.1
- Accessed api/models/contact.model.js
- Line to update: const Contact = sequelize.define("contact", {...
- Add: address: { type: Sequelize.STRING}
- Upon testing, constraints were found in DB as it seemed to be orphaned
  Encountered PostgreSQL sequence conflict with `contacts_id_seq` due to orphaned metadata.
- Verified that no other relations exist in the DB, confirming the sequence was safe to drop.
- Dropped sequence manually via `DROP SEQUENCE contacts_id_seq;` inside the DB container.
- 'Address' feature did not exist in Contacts table
- Manually inputted by accessing postgresql and typing: ALTER TABLE contacts ADD COLUMN address character varying(255);


### 3.2
- Accessed api/models/phone.model.js
- Line to update: const Phone = sequelize.define("phone", {...
- Add: phone_type: {type: Sequelize.STRING},
- Replace 'number' to 'phone_number'
- Add and renamed database columns to match task requirements:
  - `ALTER TABLE phones ADD COLUMN "phoneType" TO phone_type;`
  - `ALTER TABLE phones RENAME COLUMN "number" TO phone_number;`
- Updated and added api/models/phone.model.js to use matching name format:
  - Add  `phone_type`  
  - Update `number` → `phone_number`
- Updated api/controllers/phone.controller.js to use matching name format
  
### 3.3
- Accessed frontend/src/components/NewContact.js
- Line to update: function NewContact(props) {...
- Add const [address, setAddress] = useState('');
- Add ', address' to body: JSON.stringify({...
- Add setAddress(''); to if (data.id) {
- Add <input type='text' placeholder='Address' onChange={(e) => setAddress(e.target.value)} value={address}/> to return (...
- Accessed frontend/src/components/NewPhone.js
- Updated NewPhone.js to send phone_number and phone_type to match backend

  Front end:
  <img width="463" height="400" alt="image" src="https://github.com/user-attachments/assets/12b30547-1d4b-404d-87d9-a0e636001379" />
  Back end:
  <img width="765" height="373" alt="image" src="https://github.com/user-attachments/assets/5f2ac304-f15a-4644-a494-8ce9e03bfe56" />
  PostgreSQL DB:
  <img width="1008" height="217" alt="image" src="https://github.com/user-attachments/assets/241b8218-c8cc-4a94-b870-a59bd2c43a8a" />
  
### 3.4
#### POST Contact  
<img width="1225" height="346" alt="image" src="https://github.com/user-attachments/assets/961e053f-80ca-482b-853f-0251a888aa71" />
<img width="437" height="293" alt="image" src="https://github.com/user-attachments/assets/3a140f23-b743-42ac-8e25-d7e6c6dbfe65" />
#### PUT Contact
<img width="1280" height="283" alt="image" src="https://github.com/user-attachments/assets/108978f2-c9fe-4c08-b36a-2fd81bb589a5" />
<img width="796" height="348" alt="image" src="https://github.com/user-attachments/assets/8c923db1-c81a-43f8-be00-e1901d8a04b1" />
<img width="448" height="250" alt="image" src="https://github.com/user-attachments/assets/56978f37-9051-4238-b232-79ed2557c000" />
#### DELETE Contact
<img width="818" height="279" alt="image" src="https://github.com/user-attachments/assets/4ab88574-66ca-47f3-a95d-005766bcdb8f" />
#### GET Contact
<img width="787" height="246" alt="image" src="https://github.com/user-attachments/assets/ba6c80b1-a264-44c5-bb8d-1ff0eb68443c" />

#### POST Phone
<img width="1254" height="351" alt="image" src="https://github.com/user-attachments/assets/0269dbd2-9ed8-43c8-97e0-d50307a226c9" />
<img width="1293" height="387" alt="image" src="https://github.com/user-attachments/assets/80f2b496-ad5a-411f-aeba-941bbe9b4d34" />
#### PUT Phone
<img width="1283" height="280" alt="image" src="https://github.com/user-attachments/assets/551da152-787b-439a-9884-e3647d57f571" />
#### GET Phone
<img width="877" height="385" alt="image" src="https://github.com/user-attachments/assets/5cc575e5-1adb-4395-bdc0-4cae184090db" />
#### DELETE Phone
<img width="894" height="277" alt="image" src="https://github.com/user-attachments/assets/c721f738-541f-47d6-97c2-099959ed4018" />


## Task 4
### 4.1
-  Create the companies model - api/models/company.model.js
-  Add db.sequelize in index.js db.companies = require("./company.model.js")(sequelize, Sequelize);
-  Create companies table in Postgresql: CREATE TABLE companies (
  company_id SERIAL PRIMARY KEY,
  company_name VARCHAR(255),
  company_address VARCHAR(255),
  contact_id INTEGER REFERENCES contacts(id),
  "createdAt" TIMESTAMP WITH TIME ZONE NOT NULL,
  "updatedAt" TIMESTAMP WITH TIME ZONE NOT NULL);
<img width="749" height="81" alt="image" src="https://github.com/user-attachments/assets/7b6d957a-54cf-44b6-84d1-a630a4f3ad40" />

### 4.2
#### POST company

<img width="1634" height="749" alt="image" src="https://github.com/user-attachments/assets/b3f7005d-c9bd-412a-9f7b-af04e4ceae9f" />

#### PUT company

<img width="1501" height="278" alt="image" src="https://github.com/user-attachments/assets/a4badd05-522b-4cc1-8aed-e40973a708e0" />

#### GET company

<img width="779" height="403" alt="image" src="https://github.com/user-attachments/assets/330a6cf8-41e9-4d86-98f3-83e7bb228cda" />

#### DELETE company

<img width="823" height="279" alt="image" src="https://github.com/user-attachments/assets/5ef0d896-2846-4efc-a403-38455b352143" />

## Task 5
### 5.1
- Created CompanyList.js for displaying companies
- Created NewCompany.js for adding companies  
- Created Company.js with inline editing and delete functionality
- Integrated company management into main App.js
### 5.2
<img width="601" height="189" alt="image" src="https://github.com/user-attachments/assets/4a66a421-1299-44d1-92ba-0b62ff7be3eb" />
<img width="1077" height="102" alt="image" src="https://github.com/user-attachments/assets/b85b4375-37f5-42e4-842b-eade87b9bba8" />
<img width="783" height="409" alt="image" src="https://github.com/user-attachments/assets/01db7bc5-c1fc-45b1-9894-2d74a223ed2b" />

### 5.3
README is this one seen throughout!
  















### Phone API
