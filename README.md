

# **Chef Workstation Setup on Amazon Linux 2025**
This document provides a comprehensive guide for installing and setting up Chef on **Amazon Linux 2025**.

## **1. Launch EC2 and Connect via SSH**
- Launch an **Amazon EC2 instance** running **Amazon Linux 2025**.
- Connect to the instance using SSH:

```bash
ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
```

---

## **2. Update the System**
Ensure that all system packages are up-to-date:

```bash
sudo yum update -y
```

---

## **3. Install Git and Clone the Repository**
### **Install Git (if not installed)**
```bash
sudo yum install git -y
```

### **Clone the Repository**
Ensure the repository exists on GitHub, then run:

```bash
git clone git@github.com:Rakesh02Kumar/chef-installation.git
cd chef-installation
```

---

## **4. Download and Install Chef Workstation**
### **Download the Chef Workstation Package**
```bash
wget https://packages.chef.io/files/stable/chef-workstation/24.4.1064/el/8/chef-workstation-24.4.1064-1.el8.x86_64.rpm
```

### **Install Chef Workstation**
```bash
sudo yum install chef-workstation-24.4.1064-1.el8.x86_64.rpm
```

---

## **5. Verify the Installation**
Confirm that Chef Workstation is installed correctly:

```bash
chef --version
```

### **Troubleshooting**
If you encounter an error related to `libcrypt.so.1`, install the missing library:

```bash
sudo yum install libxcrypt-compat -y
```

---

## **6. Set Up a Chef Repository**
### **Create a Chef Repository**
```bash
chef generate repo chef-repo
cd chef-repo
```

### **Create a Cookbook**
```bash
chef generate cookbook cookbooks/test_cookbook
cd cookbooks/test_cookbook
```

### **Create a Recipe**
```bash
chef generate recipe test_recipe
```

### **Write a Recipe**
Edit the `recipes/test_recipe.rb` file:

```bash
nano recipes/test_recipe.rb
```

Add the following content:

```ruby
file '/tmp/greeting.txt' do
  content 'Hello, Chef!'
end
```

Save and exit (`CTRL + X`, then `Y` and `Enter`).

---

## **7. Run Chef Client in Local Mode**
Test your cookbook by executing Chef in local mode:

```bash
sudo chef-client --local-mode --runlist recipe[test_cookbook::test_recipe]
```

Verify the output:

```bash
cat /tmp/greeting.txt
```

---

# **Automation Script for Setup**
Save the following script as `setup_chef_project.sh` to automate the process.

```bash
#!/bin/bash

# Define variables
CHEF_WORKSTATION_URL="https://packages.chef.io/files/stable/chef-workstation/24.4.1064/el/8/chef-workstation-24.4.1064-1.el8.x86_64.rpm"
REPO_NAME="chef-repo"
COOKBOOK_NAME="test_cookbook"
RECIPE_NAME="test_recipe"
RECIPE_PATH="$REPO_NAME/cookbooks/$COOKBOOK_NAME/recipes/$RECIPE_NAME.rb"

# Update the system
echo "Updating system packages..."
sudo yum update -y

# Install Git if not installed
echo "Installing Git..."
sudo yum install git -y

# Download and install Chef Workstation
echo "Downloading Chef Workstation..."
wget $CHEF_WORKSTATION_URL -O chef-workstation.rpm

echo "Installing Chef Workstation..."
sudo yum install chef-workstation.rpm

# Verify installation
echo "Verifying Chef Workstation installation..."
chef --version

# Troubleshoot missing library issue
if ! sudo chef --version &> /dev/null; then
    echo "Encountered error with missing library. Installing compatibility library..."
    sudo yum install libxcrypt-compat -y
fi

# Set up Chef repository
echo "Setting up Chef repository..."
chef generate repo $REPO_NAME
cd $REPO_NAME

# Create a cookbook
echo "Creating a cookbook..."
chef generate cookbook cookbooks/$COOKBOOK_NAME
cd cookbooks/$COOKBOOK_NAME

# Create a recipe
echo "Creating a recipe..."
chef generate recipe $RECIPE_NAME

# Write a recipe
echo "Writing a recipe..."
cat <<EOF > recipes/$RECIPE_NAME.rb
file '/tmp/greeting.txt' do
  content 'Hello, Chef!'
end
EOF

# Run Chef Client in local mode
echo "Running Chef Client in local mode..."
sudo chef-client --local-mode --runlist recipe[$COOKBOOK_NAME::$RECIPE_NAME]

# Verify output
echo "Verifying output..."
cat /tmp/greeting.txt

echo "Chef project setup complete."
```

---

## **How to Run the Automation Script**
1. **Save the script** as `setup_chef_project.sh`.
2. **Make it executable**:

   ```bash
   chmod +x setup_chef_project.sh
   ```

3. **Run the script**:

   ```bash
   ./setup_chef_project.sh
   ```

-----------

This **fully automated script** installs **Chef Workstation**, sets up a **Chef repository**, generates a **cookbook and recipe**, and runs the **Chef client** in **local mode**. 🚀 Let me know if you need any modifications!
