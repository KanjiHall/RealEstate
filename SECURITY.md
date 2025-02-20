Here is a Security MD report for the repository owner regarding the SQL Injection vulnerability:

---

### ⚠ Security Report: SQL Injection Vulnerability

**Repository:** [RealEstate](https://github.com/spark97/RealEstate)  
**Affected File:** [modifyupload.php](https://github.com/spark97/RealEstate/blob/master/modifyupload.php)  
**Vulnerable Line:** (https://github.com/spark97/RealEstate/blob/master/modifyupload.php))

---

### 📌 Description of the Vulnerability
The file `modifyupload.php` contains a critical **SQL Injection vulnerability** due to the direct concatenation of user-supplied input (`$_GET['id']`) into SQL queries without proper sanitization or parameterization. This allows an attacker to manipulate the database queries and execute arbitrary SQL commands.

#### **Vulnerable Code:**
```php
$id=$_GET['id'];
$del="delete from flat where id='$id'";
$con=con();
$res=$con->query($del);

$query="update notification set sho ='0' where houseid='$id'";
$con1=con();
$res1=$con1->query($query);

$query2="update whishlist set sho ='0' where houseid='$id'";
$con2=con();
$res2=$con2->query($query2);
```

---

### 🔥 **Security Risk**
If an attacker crafts a malicious request like:
```
https://example.com/modifyupload.php?id=1'; DROP TABLE flat; --
```
This could lead to **data loss** (e.g., deleting the entire `flat` table), unauthorized data modifications, or even **compromising sensitive information**.

---

### ✅ Recommended Fix
To prevent SQL Injection, **use prepared statements with bound parameters**. Modify the code as follows:

```php
$id = $_GET['id'];
$con = con();
$stmt = $con->prepare("DELETE FROM flat WHERE id = ?");
$stmt->bind_param("i", $id);
$stmt->execute();
$stmt->close();

$con1 = con();
$stmt1 = $con1->prepare("UPDATE notification SET sho = '0' WHERE houseid = ?");
$stmt1->bind_param("i", $id);
$stmt1->execute();
$stmt1->close();

$con2 = con();
$stmt2 = $con2->prepare("UPDATE whishlist SET sho = '0' WHERE houseid = ?");
$stmt2->bind_param("i", $id);
$stmt2->execute();
$stmt2->close();
```
---
### 🎯 **Additional Security Measures**
1. **Validate and Sanitize Input**: Ensure `id` is an integer before processing.
2. **Use Least Privilege Principle**: Restrict database permissions to limit potential damage.
3. **Enable Error Handling**: Prevent SQL errors from being displayed to users.

---

### ⚠ **Action Required**
This issue should be **fixed immediately** to prevent potential exploitation. Consider implementing proper **prepared statements** and **input validation** throughout the application.

For any questions or assistance, feel free to reach out.

---
🚨 **Severity: HIGH** | 🛡 **Recommended Action: Immediate Fix** 🚨  
Thanks for addressing this issue to improve the security of your application.  

