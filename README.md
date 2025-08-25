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




### Phone API
