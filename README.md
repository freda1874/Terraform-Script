<h1 align="center"> Terraform with AWS Infrastructure Setup</h1>

<p>
  This project demonstrates how to use <b>Terraform</b> to define, provision, and manage AWS infrastructure using Infrastructure as Code (IaC).
  It covers core Terraform functionalities such as initializing, applying, and managing state files, as well as deploying an <b>Ubuntu EC2 instance</b> with networking configurations.
</p>
<h2> Key Concepts Practiced</h2>
 
<ul>
  <li><b>Terraform</b>: Automates AWS infrastructure provisioning.</li>
  <li><b>Infrastructure as Code (IaC)</b>: Manages cloud resources via code.</li>
  <li><b>State Management</b>: Tracks deployed resources.</li>
  <li><b>EC2 Deployment</b>: Launches an AWS instance with Terraform.</li>
  <li><b>Networking</b>: Configures VPC, subnets, route tables, and security groups.</li>
</ul>

 
![image](https://github.com/user-attachments/assets/c391449a-adc0-4a43-9268-1a556c6f2ff8)


<h2>⚙️ Setting Up Terraform</h2>

<b>🔧 Prerequisites</b>
<ul>
  <li>Install <b>Terraform</b>: <code>brew install terraform</code> (Mac) or <code>choco install terraform</code> (Windows).</li>
  <li>Ensure you have an <b>AWS account</b> and AWS CLI configured: <code>aws configure</code></li>
  <li>Set up an SSH key pair in AWS to access EC2 instances.</li>
</ul>

<b>🚀 Essential Terraform Commands</b>

<table align="center" border="1" cellpadding="6" cellspacing="0">
  <tr>
    <th>Action</th>
    <th>Command</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Initialize Terraform</td>
    <td><code>terraform init</code></td>
    <td>Initializes Terraform in the working directory.</td>
  </tr>
  <tr>
    <td>Preview Changes</td>
    <td><code>terraform plan</code></td>
    <td>Shows the changes Terraform will apply before execution.</td>
  </tr>
  <tr>
    <td>Apply Changes</td>
    <td><code>terraform apply</code></td>
    <td>Creates or updates AWS infrastructure as defined in the code.</td>
  </tr>
  <tr>
    <td>Destroy Infrastructure</td>
    <td><code>terraform destroy</code></td>
    <td>Deletes all Terraform-managed resources.</td>
  </tr>
  <tr>
    <td>List Tracked Resources</td>
    <td><code>terraform state list</code></td>
    <td>Displays all resources tracked in the Terraform state file.</td>
  </tr>
  <tr>
    <td>Show Details of a Resource</td>
    <td><code>terraform state show aws_instance.web-server</code></td>
    <td>Shows the current state and attributes of a specific resource.</td>
  </tr>
  <tr>
    <td>Rename/Move a Resource</td>
    <td><code>terraform state mv aws_instance.old-name aws_instance.new-name</code></td>
    <td>Renames or moves a resource within the Terraform state.</td>
  </tr>
  <tr>
    <td>Remove a Resource Without Deleting It</td>
    <td><code>terraform state rm aws_s3_bucket.my_bucket</code></td>
    <td>Stops tracking a resource in Terraform without destroying it.</td>
  </tr>
</table>

<b> Terraform Output Variables</b>

Retrieve EC2 Public IP
<p>Add the following to <code>main.tf</code> to output the EC2 public IP:</p>
<pre>
output "instance_public_ip" {
  description = "Public IP of the EC2 instance"
  value       = aws_instance.web-server.public_ip
}
</pre>

<p>Run this command to display the value:</p>
<pre>
terraform output instance_public_ip
</pre>

---
<h2> Deploying an AWS EC2 Instance</h2>

<h3>Terraform Configuration for an EC2 Instance</h3>
<pre>
resource "aws_instance" "web-server-instance" {
  ami               = "ami-04b4f1a9cf54c11d0"
  instance_type     = "t2.micro"
  availability_zone = "us-east-1a"
  key_name          = "terraform-main-key"

  network_interface {
    device_index         = 0
    network_interface_id = aws_network_interface.web-server-nic.id
  }

  user_data = <<-EOF
                #!/bin/bash
                sudo apt update -y
                sudo apt install apache2 -y
                sudo systemctl start apache2
                sudo bash -c 'echo "Your very first web server" > /var/www/html/index.html'
                EOF
  tags = {
    Name = "web-server"
  }
}
</pre>

<h2> Networking Setup: VPC, Subnets, Security Groups</h2>

![image](https://github.com/user-attachments/assets/b73b139e-0f1a-40f9-87f0-74972241101d)

<h3> VPC & Subnet Configuration</h3>
<p>It is recommended to use the same Availability Zone (AZ) as the subnet.</p>
<ul>
  <li><b>VPC</b>: Defines a private network in AWS.</li>
  <li> <b>Subnet</b>: Divides a VPC into isolated networks.</li>
  <li> <b>Internet Gateway</b>: Allows outbound internet traffic from the VPC.</li>
  <li><b>Route Table</b>: Controls traffic routing within the VPC.</li>
  <li><b>Security Groups</b>: Control inbound/outbound traffic to instances.</li>
</ul>

<h3>Accessing EC2 on Mac/Linux</h3>
<pre>
ssh -i "your-key.pem" ubuntu@<instance-public-ip>
</pre>

<h3>Accessing EC2 on Windows (Using PuTTY)</h3>
<ul>
  <li>1️⃣ Download and install <b>PuTTY</b>.</li>
  <li>2️⃣ Open <b>PuTTYgen</b> → Load your <code>.pem</code> key → Save as <code>.ppk</code>.</li>
  <li>3️⃣ Open <b>PuTTY</b> → Enter Hostname: <code>ubuntu@34.232.56.18</code>.</li>
  <li>4️⃣ Under SSH → Auth → Select your <code>.ppk</code> key.</li>
  <li>5️⃣ Click **Open** to connect.</li>
</ul>

 

<h2>📜 Credit</h2>

<p>Inspired by this tutorial: https://www.youtube.com/watch?v=SLB_c_ayRMo </p>

 
</p>
