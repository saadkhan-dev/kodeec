## Kodeec
A full production backend API built with these tech stacks:

- REST API: _Flask and Flask-RESTFul_.
- Database: _PostgresSQL_.
- Unit Testing: _Pytest_.
- Packaging Management: _Poetry_.
- Containerization: _Docker and Docker Compose_.
- Cloud Provider: _Microsoft Azure:_
  - _Azure VNet (Virtual Network)._
  - _Azure Virtual Machine._
  - _Azure Database for PostgreSQL._
  - _Azure Blob Storage._
  - _Azure Container Registry (ACR)._
- Infrastructure as Code: _Terraform_.
- CI/CD: _GitHub Actions_.

---

### Backend:

**Set the environment variables:**
- Copy `backend/.env.sample/` folder and rename it to `backend/.env/`.

**Run the base environment locally:**
- Update the `backend/.env/.env.base` file.
- Run Docker Compose:
  ```shell
  docker compose -f backend/.docker-compose/base.yml up -d --build
  ```
- Run Pytest:
  ```shell
  docker exec -it kodeec_base_flask /bin/bash -c "/opt/venv/bin/pytest"
  ```

---

### Infrastructure:

**Setup Terraform Backend:**
- Create a storage on Azure Blob Storage.
- Create a file and name it to `.backend.hcl` under `infrastructure` folder.
- Copy the content of file `.backend.hcl.sample` inside it and fill the values.

**Setup Secrets:**
- Create a file with the name `.secrets.auto.tfvars` under `infrastructure` folder.
- Copy the contents of file `.secrets.auto.tfvars.sample` inside it and fill the values.

**Setup SSH:**
- Generate an SSH Key.
- Create a folder with the name `.ssh` under `infrastructure` folder.
- Copy `id_rsa.pub` and `id_rsa` file to `infrastructure/.ssh`.

**Run Terraform Commands:**

- terraform init
  ```shell
  docker compose -f infrastructure/.docker-compose.yml run --rm terraform init -backend-config=.backend.hcl
  ```

-
- terraform plan all
  ```shell
  docker compose -f infrastructure/.docker-compose.yml run --rm terraform plan
  ```
- terraform plan azure
  ```shell
  docker compose -f infrastructure/.docker-compose.yml run --rm terraform plan -target="module.azure"
  ```

-
- terraform apply all
  ```shell
  docker compose -f infrastructure/.docker-compose.yml run --rm terraform apply --auto-approve
  ```
- terraform apply azure
  ```shell
  docker compose -f infrastructure/.docker-compose.yml run --rm terraform apply -target="module.azure" --auto-approve
  ```

- 
- terraform destroy all
  ```shell
  docker compose -f infrastructure/.docker-compose.yml run --rm terraform destroy --auto-approve
  ```
- terraform destroy azure
  ```shell
  docker compose -f infrastructure/.docker-compose.yml run --rm terraform destroy -target="module.azure" --auto-approve
  ```

- 
- terraform output azure
  ```shell
  docker compose -f infrastructure/.docker-compose.yml run --rm terraform output azure
  ```

---

