# Setting up a Virtual Box Development Environment

## Prepare related Virtual box image vagrant (BOX = "generic/ubuntu1804")

## Install GP
***The following steps run inside the virtual box***
0. Prepare environment
```Bash
sudo apt-get install -y build-essential
sudo apt-get install python
```

1. Prepare directory
```Bash
sudo mkdir -p /src/gpdb_src
sudo chown $(id -u):$(id -g) /src/gpdb_src
```

2. Clone repo and add upstream to origin (be sure you have correct ssh-key to repo access)
```Bash
git clone git@github.com:vkhashkovsky/gpdb.git /src/gpdb_src
cd /src/gpdb_src
git remote add upstream git@github.com:greenplum-db/gpdb.git
git checkout 6X_STABLE
```

3. Create user: gpadmin (Steps from README.linux.md  -> Common Platform Tasks:)
```Bash
cd /src
sudo ./gpdb_src/concourse/scripts/setup_gpadmin_user.bash
```
3.1 Add authorized_key for user: gpadmin
```Bash
ssh-keygen -t rsa -P "" -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub | sudo tee -a /home/gpadmin/.ssh/authorized_keys
//check accessibility and accept host key checking
ssh gpadmin@127.0.0.1 -i ~/.ssh/id_rsa
```

4. Configure and build (Steps from README.linux.md  -> For Ubuntu:)
```Bash
cd /src/gpdb_src
sudo chmod 777 /usr/local
./configure --disable-orca --with-perl --with-python --with-libxml --prefix=/usr/local/gpdb
make -j8
make -j8 install
source /usr/local/gpdb/greenplum_path.sh
```

5. Start cluster
```Bash
cd /src/gpdb_src
make create-demo-cluster
```
