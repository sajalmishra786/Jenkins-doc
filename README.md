
# Setting Up Jenkins on an AWS VM

This guide walks you through setting up Jenkins on an AWS EC2 instance, from selecting the right instance to installing and securing Jenkins.

---

## 1. Choose an AWS VM

1. **Log in to AWS Management Console:**
   - Navigate to the EC2 dashboard.
2. **Launch an EC2 Instance:**
   - Click "Launch Instances."
   - Select an Amazon Machine Image (AMI). For Jenkins, consider **Ubuntu Server 22.04 LTS** or **Amazon Linux 2**.
   - Choose an instance type like **t2.medium**, which offers moderate performance.
3. **Configure Instance Details:**
   - Set the number of instances to 1.
   - Adjust details like VPC, subnet, and IAM roles based on your needs.
4. **Add Storage:**
   - Assign enough storage, typically 20 GB.
5. **Add Tags:**
   - Add a tag, such as `Name: Jenkins-Server`.
6. **Review and Launch:**
   - Use or create a key pair for SSH access.
   - Launch the instance.

---

## 2. Configure Security Groups

1. **Create a Security Group:**
   - Go to the **EC2 dashboard** and select **Security Groups**.
   - Create a new security group, e.g., `Jenkins-SG`.
2. **Set Inbound Rules:**
   - Allow traffic on **port 8080** from your IP address or CIDR to access Jenkins.
   - Optionally, allow **SSH (port 22)** for remote access.
3. **Set Outbound Rules:**
   - Permit all outbound traffic or restrict it as needed.
4. **Associate Security Group:**
   - Attach this security group to your EC2 instance.

**Log In to EC2 Instance Using Git Bash:**

```bash
ssh -i /path/to/your-key.pem ubuntu@<EC2-Public-IP>
```

---

## 3. Install Jenkins

1. **Update the System:**
```bash
sudo apt update && sudo apt upgrade -y
```
2. **Add Jenkins Repository:**
```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee "/usr/share/keyrings/jenkins.asc" > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```
3. **Install Jenkins:**
```bash
sudo apt update
sudo apt install jenkins -y
```
4. **Start Jenkins Service:**
```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
**Check Jenkins Status:**
```bash
sudo systemctl status jenkins
```
This verifies whether Jenkins is running.

---

## 4. Secure Jenkins

1. **Access Jenkins Interface:**
   - Open your browser and navigate to `http://<EC2-Public-IP>:8080`.
2. **Unlock Jenkins:**
   - Retrieve the initial admin password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
3. **Set Up Admin User:**
   - Follow the on-screen instructions to create an admin user with a strong password.
4. **Configure Security:**
   - Go to **`Manage Jenkins > Configure Global Security`**.
   - Enable Role-Based Authorization and set roles and permissions.
5. **Install Plugins:**
   - During setup, install recommended plugins for Jenkins functionality.

---

## 5. Test and Verify

1. **Verify Jenkins Access:**
   - Log in to the Jenkins web interface using the admin credentials you set up.
2. **Run a Test Job:**
   - Create and execute a simple freestyle project.
3. **Confirm Functionality:**
   - Ensure Jenkins can execute jobs and the plugins are functioning correctly.

**Test Connectivity:**

```bash
curl -I http://<EC2-Public-IP>:8080
```
This checks whether Jenkins is accessible at the specified URL.

---

## Conclusion
This guide simplifies the process of setting up Jenkins on an AWS EC2 instance. By following these steps, you’ll have a fully functional and secure Jenkins environment to support your CI/CD workflows.
