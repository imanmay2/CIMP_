# 🌐 CIMP - Club Information Management Portal (Backend)

This is the **Node.js + Express.js + MongoDB backend** for the Club Information Management Portal (CIMP). It supports user authentication, club management, member approval system, and role-based access for **Admin**, **Faculty**, and **Student (President)**.

---

## 🚀 Tech Stack

- **Backend Framework**: Express.js
- **Database**: MongoDB (Mongoose ODM)
- **Authentication**: Bcrypt.js, Cookies
- **Environment Variables**: dotenv
- **CORS Handling**: cors
- **Session Tracking**: cookie-parser

---

## 📁 Project Structure

```
models/
│
├── Admin.cjs       // Admin schema
├── Faculty.cjs     // Faculty schema
├── Student.cjs     // Student/President schema
├── User.cjs        // User schema with role
├── Request.cjs     // Member request schema
├── clubs.cjs       // Club schema
```

---

## ⚙️ How it Works

- Users sign up as **admin**, **faculty**, or **president**.
- Depending on role, data is saved to respective collections.
- Clubs are created by Admins and associated with a president and faculty.
- Students request to join clubs. Requests go to Admin.
- Admin approves/rejects requests. Members are updated accordingly.

---

## 🌐 API Endpoints

### ✅ `GET /getData`
Returns a test response to confirm the server is running.

```json
{ "message": "Hello From Backend !! " }
```

---

### 📝 `POST /signUp`
Registers a new user.

#### Request Body
```json
{
  "Name": "John Doe",
  "Email": "john@gmail.com",
  "Password": "123456",
  "Role": "faculty",
  "id": "FAC123"
}
```

#### Logic
- Validates `@gmail.com` emails only.
- Password is hashed using bcrypt.
- Creates user in `User` collection and respective role-based collection.
- Sets cookies: `login`, `id`.

#### Responses
- Success: `flag: "success"`
- User Exists / Invalid Email: `flag: "error"`

---

### 🔐 `POST /login`
Authenticates an existing user.

#### Request Body
```json
{
  "Email": "john@gmail.com",
  "Password": "123456"
}
```

#### Responses
- On success:
```json
{
  "message": "User Logged in Successfully",
  "flag": "success",
  "role": "faculty"
}
```
- On failure: `flag: "error"`

---

### 🧿 `POST /createNewClub`
Creates a new club.

#### Request Body
```json
{
  "name": "Tech Club",
  "president": "REG123",
  "facultyCoordinator": "FAC123",
  "maxMemberCount": 30,
  "category": "Technical",
  "status": "Active"
}
```

#### Responses
- Club Created: `flag: "success"`
- Club Exists / President or Faculty Not Found: `flag: "error"`

---

### 📄 `GET /getClubData`
Fetches all club data with related faculty and president details.

#### Response Example
```json
[
  {
    "_id": "...",
    "name": "Tech Club",
    "presidentId": "REG123",
    "president": "John Doe",
    "presidentEmail": "john@gmail.com",
    "facultyId": "FAC123",
    "faculty": "Dr. Smith",
    "members": [...],
    "maxMemberCount": 30,
    "category": "Technical",
    "status": "Active"
  }
]
```

---

### 👥 `GET /getMembers/:president_id`
Returns members under a given president’s club.

#### Example
```json
{
  "data_": [{ "regNo": "REG001", "name": "Alice" }],
  "flag": "success",
  "faculty_id": "FAC123",
  "clubName": "Tech Club"
}
```

---

### 👤 `GET /getInfo/:president_id/:faculty_id`
Returns details of both president and faculty.

```json
{
  "data_": [
    { "_id": "...", "regNo": "FAC123", "name": "Dr. Smith", "role": "Faculty", "email": "smith@gmail.com" },
    { "_id": "...", "regNo": "REG123", "name": "John", "role": "President", "email": "john@gmail.com" }
  ],
  "flag": "success"
}
```

---

### 📥 `POST /addNewMember`
Creates a request to join a club.

#### Request Body
```json
{
  "presidentName": "John",
  "clubName": "Tech Club",
  "members": [
    { "regNo": "REG789", "name": "Alice" },
    { "regNo": "REG456", "name": "Bob" }
  ]
}
```

#### Response
```json
{ "message": "Member Added Successfully", "flag": "success" }
```

---

### 📬 `GET /getNewMembers`
Returns all pending join requests.

```json
{
  "data_": [
    {
      "_id": "...",
      "clubName": "Tech Club",
      "presidentName": "John",
      "members": [{ "regNo": "REG456", "name": "Bob" }],
      "date": "Jul 4, 10:30 AM",
      "status": "Pending"
    }
  ],
  "flag": "success"
}
```

---

### ✅ `POST /requestApproval/:id`
Approves or rejects a join request.

#### URL Param
- `:id` — The Request document ID

#### Request Body
```json
{
  "status": "Approved" // or "Rejected"
}
```

If approved, members are added to the `Club` document.

#### Response
```json
{ "message": "Member Added Successfully", "flag": "success" }
```

---

### 🚪 `GET /logout`
Logs out the user by clearing cookies.

#### Response
```json
{ "message": "Logged Out Sucessfully", "flag": "success" }
```

---

## 📦 Installation & Setup

```bash
git clone https://github.com/your-username/CIMP-backend.git
cd CIMP-backend
npm install
```

Create `.env`:
```
PORT=8080
```

Start server:
```bash
node index.js
```



## 🧑‍💻 Author

**Manmay Chakraborty**  
B.Tech CSE, VIT Chennai  
Email: _imanmay2@gmail.com_

---

## 🛡️ License

This project is licensed under the MIT License.
