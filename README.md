# <div align="center">**Scalable Distributed Processing Engine via MPI and Docker**</div>

<br>

## 📝 Abstract

<div align="justify">

NexusMPI presents a scalable solution for analyzing large-scale text files by counting the frequency of words and characters. The system leverages MPI (Message Passing Interface) to enable parallel processing across multiple nodes and evaluates performance in both native environments and containerized Docker environments. The implementation is designed to measure scalability by varying the number of processes or containers, providing insights into execution efficiency under distributed computing frameworks. NexusMPI emphasizes not only correct word and character frequency analysis but also evaluates the impact of containerization on parallel computing performance.

</div>

---

## 🛠️ Technologies Used

<div align="justify">

NexusMPI utilizes several technologies and frameworks to enable parallel execution, distributed computing, and containerization. A combination of open-source tools is employed to build a scalable and efficient distributed processing engine.

</div>

- Python 3
- MPI (Message Passing Interface) — OpenMPI implementation
- Docker
- Docker Compose
- Ubuntu 20.04 (Base Operating System)

---

## ⚙️ Installation and Setup

<div align="justify">

This section describes the necessary steps to set up the development and execution environment for NexusMPI. It includes installing Docker, generating SSH keys, building the Docker image, and setting up containers for parallel execution using MPI.

</div>

### Install Docker and Docker Compose

Follow the commands below to install Docker and Docker Compose on Ubuntu:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-compose
```

Add the user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

After adding, log out and log back in to apply the changes.

Verify installation:

```bash
docker ps
docker run --rm hello-world
```

### Generate SSH Keys for MPI Communication

Inside your project directory:

```bash
mkdir workspace
cd workspace
mkdir .ssh
ssh-keygen -t rsa -f $(pwd)/.ssh/id_rsa -q -N ""
cat $(pwd)/.ssh/id_rsa.pub >> $(pwd)/.ssh/authorized_keys
```

### Build the Docker Image

Build the MPI-enabled Docker image:

```bash
docker build -t nexusmpi-docker .
```

Verify the image creation:

```bash
docker images
```

### Start the MPI Docker Containers

Use Docker Compose to start the containers:

```bash
docker-compose up -d
```

Verify the running containers:

```bash
docker ps
```

### Create the Hosts File

Create a hosts file for MPI to recognize the available nodes:

```bash
echo "mpi-node1 slots=1" > hosts
echo "mpi-node2 slots=1" >> hosts
```

### Verify MPI Communication

Execute the following to test MPI communication between containers:

```bash
docker exec -u mpi mpi-node1 mpirun -np 2 -hostfile /workspace/hosts hostname
```

Expected output:

```bash
mpi-node1
mpi-node2
```

---

## 🚀 Compilation and Running Instructions

### Native Environment Execution

Run the following command to execute the program natively using MPI:

```bash
mpirun -np <number_of_processes> python3 parcount.py <input_file>
```

Example:

```bash
mpirun -np 4 python3 parcount.py test1.txt
```

- `<number_of_processes>`: Number of MPI processes (e.g., `1`, `2`, `4`, `6`, `8`)
- `<input_file>`: Text file for analysis (e.g., `test1.txt`, `test2.txt`, etc.)

### Docker Environment Execution

Execute the program inside the Docker containers as follows:

```bash
docker exec -u mpi mpi-node1 mpirun -np <number_of_containers> -hostfile /workspace/hosts python3 /workspace/parcount.py /workspace/<input_file>
```

Example:

```bash
docker exec -u mpi mpi-node1 mpirun -np 4 -hostfile /workspace/hosts python3 /workspace/parcount.py /workspace/test5.txt
```

- `<number_of_containers>`: Number of containers participating in execution (e.g., `1`, `2`, `4`, `6`, `8`)
- `<input_file>`: Text file located in `/workspace/`

### Stopping and Cleaning Up

To stop the running Docker containers after the experiments:

```bash
docker-compose down
```

This command will stop and remove all the containers created by Docker Compose.

---

## 📊 Results, Analysis, and Complexity

<div align="justify">

For detailed performance results, complexity analysis, scalability observations, and additional experimental outputs, please feel free to contact me.

</div>

---

## 📬 Contact Information

- **Email**: manne.bharadwaj.1953@gmail.com
- **LinkedIn**: [Bharadwaj Manne](https://www.linkedin.com/in/bharadwaj-manne-711476249/)
