chef-workstation-setup-amazon-linux-2025
Basic Chef Workstation Project

This document provides a comprehensive guide for installing and setting up Chef on Amazon Linux 2025. Follow these instructions to ensure proper installation and configuration.

Launch EC2 and Connect via SSH
1. Launch an Amazon EC2 instance running Amazon Linux 2025.
2. Connect to the instance using SSH.

Step 1: Update the System

Ensure that all system packages are up-to-date with the latest updates and security patches:

sudo yum update -y

To clone the repository from GitHub, follow these instructions:

Cloning the Repository

1.Ensure Git is installed on your system. If not, install it using the following command:

For Amazon Linux 2025:

sudo yum install git -y

2.Clone the repository using Git. Open your terminal and run the following command:
Clone the repository using Git. Open your terminal and run the following command:

git clone git@github.com:Rakesh02Kumar/chef-inastallation.git

3. Navigate into the cloned repository directory:

cd chef-project

Step 2: Download and Install Chef Workstation
Chef Workstation is a suite of tools for managing infrastructure and executing Chef recipes.

Download the Chef Workstation package:

wget https://packages.chef.io/files/stable/chef-workstation/24.4.1064/el/8/chef-workstation-24.4.1064-1.el8.x86_64.rpm

2. Install the downloaded package:

sudo yum install chef-workstation-24.4.1064-1.el8.x86_64.rpm

Step 3: Verify the Installation

Confirm that Chef Workstation is installed correctly by checking the version:

chef --version

Troubleshooting

If you encounter the error /opt/chef-workstation/embedded/bin/ruby: error while loading shared libraries: libcrypt.so.1: cannot open shared object file: No such file or directory, resolve it by installing the necessary compatibility library:

sudo yum install libxcrypt-compat -y

Step 4: Set Up a Chef Repository

1.Create a Chef repository:

chef generate repo chef-repo
cd chef-repo

2.Create a Cookbook:

chef generate cookbook cookbooks/test_cookbook
cd cookbooks/my_cookbook

3.create recipe:
chef generate recipe recipes/test_recipe

4.Write a Recipe:

Edit the recipes/test_recipe.rb file in your cookbook:

vim recipes/default.rb
Add the following content:

file '/tmp/greeting.txt' do
  content 'Hello, Chef!'
end

Step 5: Run Chef Client in Local Mode
Test your cookbook by executing Chef in local mode:

sudo chef-client --local-mode --runlist recipe[my_cookbook::default]
cat /tmp/greeting.txt

Automation Script run:
Here's a shell script to automate the setup and configuration process for your Chef project on Amazon Linux 2023:

#!/bin/bash

# Define variables
CHEF_WORKSTATION_URL="https://packages.chef.io/files/stable/chef-workstation/24.4.1064/el/8/chef-workstation-24.4.1064-1.el8.x86_64.rpm"
REPO_NAME="chef-repo"
COOKBOOK_NAME="my_cookbook"
RECIPE_PATH="$REPO_NAME/cookbooks/$COOKBOOK_NAME/recipes/default.rb"

# Update the system
echo "Updating system packages..."
sudo yum update -y

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

# Write a recipe
echo "Writing a recipe..."
mkdir -p recipes
cat <<EOF > recipes/default.rb
file '/tmp/greeting.txt' do
  content 'Hello, Chef!'
end
EOF

# Run Chef Client in local mode
echo "Running Chef Client in local mode..."
sudo chef-client --local-mode "recipe[test_cookbook::test_recipe]

# Verify output
echo "Verifying output..."
cat /tmp/greeting.txt

echo "Chef project setup complete."

Instructions:

1.Save the script as setup_chef_project.sh.

2.Make the script executable:

chmod +x setup_chef_project.sh

3.Run the script:

./setup_chef_project.sh

This script handles the entire process from system update to Chef Workstation installation, repository setup, cookbook creation, recipe writing, and Chef client execution. Adjust the CHEF_WORKSTATION_URL and other variables as needed.

