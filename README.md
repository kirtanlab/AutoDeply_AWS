# 🌈 RainbowText

**RainbowText** is a minimal Flask + React web application designed to demonstrate a **fully automated containerized deployment pipeline** using **Docker Compose** and **AWS ECS Fargate**. While the app itself renders a simple rainbow-colored animated text page, the focus is on teaching infrastructure-as-code, CI/CD, and cloud-native deployment workflows.

---

## 🚀 Problem It Solves

Modern application deployment often requires setting up and managing complex infrastructure. Developers and DevOps engineers need:

- A seamless way to **containerize** applications
- Automation to **push Docker images to Amazon ECR**
- A robust, serverless hosting solution like **AWS ECS Fargate**
- Scripts that handle **IAM roles, networking, and ECS services** without manual setup

This project solves these challenges with a **plug-and-play setup** that:

- Builds your app locally or for production
- Pushes images to AWS ECR
- Creates and deploys an ECS Fargate service with minimal manual effort
- Provides teardown scripts for easy cleanup

---

## 🧠 Project Highlights

### ✨ Application Stack

- **Frontend**: React (served via Nginx)
- **Backend**: Flask with uWSGI
- **Webserver**: Nginx reverse proxy
- **Container Orchestration**: Docker Compose
- **Cloud Deployment**: AWS ECS Fargate

### 📦 Folder Structure

```text
rainbowtext/
│
├── app.py                  # Flask application entry
├── dockerfile              # Backend Dockerfile
├── docker-compose.yml      # Compose file for local dev
├── ecs-params.yml          # ECS-specific configurations
├── nginx/                  # Nginx Dockerfile & configs
│   ├── nginx.conf
│   ├── nginx.dev.conf
│   └── dockerfile
├── scripts/                # AWS deploy/destroy utilities
│   ├── deploy.sh
│   ├── destroy.sh
│   ├── setup_ecs.sh
│   ├── config_ecr.py
│   ├── login_ecr.sh
│   └── .env                # Your AWS environment variables
├── templates/home.html     # HTML file with rainbow effect
├── uwsgi.ini               # uWSGI configuration
├── requirements.txt
└── README.md
```

---

## 🛠️ Setup Instructions

### 📍 Prerequisites

- Python 3.x
- Docker & Docker Compose
- AWS CLI configured or AWS Access credentials
- AWS account with ECS and ECR access

### 🧪 Local Development

```bash
docker-compose build
docker-compose up
```

Visit [http://localhost](http://localhost) to see the rainbow text app.

---

## ☁️ Production Deployment to AWS ECS Fargate

### 🧾 Step-by-Step

1. Configure environment variables:

Create `.env` file inside `scripts/` directory:

```bash
export AWS_PROJECT=rainbowtext
export AWS_ROLE=your-role-name
export AWS_ACCOUNT_ID=your-aws-account-id
export AWS_REGION=your-region
export AWS_ACCESS_KEY_ID=your-access-key
export AWS_SECRET_ACCESS_KEY=your-secret-key
```

Then load the variables:

```bash
source scripts/.env
```

2. Install dependencies and activate Python environment:

```bash
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
```

3. Deploy:

```bash
docker-compose build --build-arg build_env="production"
./scripts/deploy.sh > deploy.log 2>&1 &
```

---

## 🌐 Accessing the Service

After deployment, extract the security group:

```bash
grep security_grp deploy.log
```

Then, go to the AWS Console → **VPC → Security Groups**, find the group, and **add an HTTP inbound rule** to allow public access.

---

## 🔁 Cleanup

To destroy the ECS cluster and remove resources:

```bash
./scripts/destroy.sh
```

---

## 📊 Evaluation

| Feature                         | Evaluation                                      |
|-------------------------------|-------------------------------------------------|
| Ease of Use                   | Plug-and-play deployment with pre-written scripts |
| Scalability                   | Uses AWS ECS Fargate for serverless scaling     |
| Security                      | IAM roles and environment-specific credentials  |
| Flexibility                   | Easily replace app with your own containerized app |
| Learning Outcome              | Demonstrates end-to-end CI/CD to AWS Fargate    |

---

## 🧩 Key Components in the Pipeline

- **login_ecr.sh**: Logs into Amazon ECR with provided credentials.
- **config_ecr.py**: Creates ECR repos and pushes Docker images.
- **setup_ecs.sh**: Provisions ECS cluster, task definitions, and deploys services.

---

## 📚 Additional Notes

- Use `instructions.txt` for deep-dive tips and AWS-specific notes.
- The AMI `ami-09712b7bf6d670b8f` can be used to quickly spin up a pre-configured AWS Linux instance with dependencies.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).