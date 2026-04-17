#  Delivery Checklist — Day 12 Lab Submission

> **Student Name:** _________________________  
> **Student ID:** _________________________  
> **Date:** _________________________

---

##  Submission Requirements

Submit a **GitHub repository** containing:

### 1. Mission Answers (40 points)

Create a file `MISSION_ANSWERS.md` with your answers to all exercises:

```markdown
# Day 12 Lab - Mission Answers

## Part 1: Localhost vs Production

### Exercise 1.1: Anti-patterns found
1. API key hardcode trong code
2. Không có config management
3. Print thay vì proper logging
4. Không có health check endpoint, nếu agent crash, platform không biết để restart
5. Port cố định — không đọc từ environment

### Exercise 1.3: Comparison table
| Feature | Basic | Advanced | Tại sao quan trọng? |
|---------|-------|----------|---------------------|
| Config | Hardcode | Env vars | Giúp bảo mật (không lộ API key lên GitHub) và dễ dàng thay đổi cấu hình cho từng môi trường (Dev, Staging, Prod) mà không cần sửa code. |
| Health check | Không có | Có /health (Liveness) và /ready (Readiness) | Giúp các hệ thống quản lý (Docker, Kubernetes) biết khi nào container bị "treo" để restart hoặc khi nào đã sẵn sàng để nhận khách. |
| Logging | print() | JSON | Giúp các hệ thống quản lý log tập trung dễ dàng tìm kiếm, lọc và phân tích lỗi thay vì phải đọc những dòng text lộn xộn. |
| Shutdown | Đột ngột | Graceful | Giúp đảm bảo rằng ứng dụng tắt một cách an toàn, không làm mất dữ liệu hoặc kết nối đang hoạt động. |

## Part 2: Docker

### Exercise 2.1: Dockerfile questions
1. Base image là gì: Base image là hình ảnh Docker mà bạn sử dụng làm nền tảng để xây dựng image của mình. Nó cung cấp hệ điều hành và các công cụ cơ bản cần thiết để chạy ứng dụng của bạn. Ví dụ, `python:3.9-slim` là một base image phổ biến cho các ứng dụng Python, cung cấp Python 3.9 trên một hệ điều hành nhẹ. 
2. Working directory là gì: Working directory là thư mục trong container mà các lệnh tiếp theo sẽ được thực thi. Khi bạn thiết lập working directory, tất cả các lệnh như RUN, CMD, COPY sẽ được thực hiện trong thư mục đó. Ví dụ, nếu bạn đặt `WORKDIR /app`, thì khi bạn chạy `COPY . .`, tất cả file sẽ được copy vào thư mục `/app` trong container. Điều này giúp tổ chức file tốt hơn và tránh việc phải chỉ định đường dẫn dài trong các lệnh sau đó. 
3. Tại sao COPY requirements.txt trước?: Để tận dụng cache của Docker. Nếu chỉ COPY source code trước, thì mỗi lần bạn thay đổi code, Docker sẽ phải cài lại dependencies, trong khi nếu COPY requirements.txt trước, Docker chỉ phải cài lại dependencies khi requirements.txt thay đổi, giúp tiết kiệm thời gian build.
4. CMD vs ENTRYPOINT khác nhau thế nào?: CMD cung cấp lệnh mặc định có thể bị ghi đè khi chạy container, trong khi ENTRYPOINT thiết lập lệnh cố định mà không thể bị ghi đè. Nếu bạn muốn cho phép người dùng chạy container với các lệnh khác nhau, bạn nên sử dụng CMD. Nếu bạn muốn đảm bảo rằng một lệnh cụ thể luôn được chạy (ví dụ: khởi động ứng dụng), bạn nên sử dụng ENTRYPOINT. Thông thường, bạn có thể kết hợp cả hai: ENTRYPOINT để chỉ định lệnh chính và CMD để cung cấp tham số mặc định.
...

### Exercise 2.3: Image size comparison
- Develop: 1.66 GB
- Production: 236 MB
- Difference: 85.8%

## Part 3: Cloud Deployment

### Exercise 3.1: Railway deployment
- URL: https://your-app.railway.app
- Screenshot: [Link to screenshot in repo]

## Part 4: API Security

### Exercise 4.1-4.3: Test results
[Paste your test outputs]

### Exercise 4.4: Cost guard implementation
[Explain your approach]

## Part 5: Scaling & Reliability

### Exercise 5.1-5.5: Implementation notes
[Your explanations and test results]
```

---

### 2. Full Source Code - Lab 06 Complete (60 points)

Your final production-ready agent with all files:

```
your-repo/
├── app/
│   ├── main.py              # Main application
│   ├── config.py            # Configuration
│   ├── auth.py              # Authentication
│   ├── rate_limiter.py      # Rate limiting
│   └── cost_guard.py        # Cost protection
├── utils/
│   └── mock_llm.py          # Mock LLM (provided)
├── Dockerfile               # Multi-stage build
├── docker-compose.yml       # Full stack
├── requirements.txt         # Dependencies
├── .env.example             # Environment template
├── .dockerignore            # Docker ignore
├── railway.toml             # Railway config (or render.yaml)
└── README.md                # Setup instructions
```

**Requirements:**
-  All code runs without errors
-  Multi-stage Dockerfile (image < 500 MB)
-  API key authentication
-  Rate limiting (10 req/min)
-  Cost guard ($10/month)
-  Health + readiness checks
-  Graceful shutdown
-  Stateless design (Redis)
-  No hardcoded secrets

---

### 3. Service Domain Link

Create a file `DEPLOYMENT.md` with your deployed service information:

```markdown
# Deployment Information

## Public URL
https://your-agent.railway.app

## Platform
Railway / Render / Cloud Run

## Test Commands

### Health Check
```bash
curl https://your-agent.railway.app/health
# Expected: {"status": "ok"}
```

### API Test (with authentication)
```bash
curl -X POST https://your-agent.railway.app/ask \
  -H "X-API-Key: YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"user_id": "test", "question": "Hello"}'
```

## Environment Variables Set
- PORT
- REDIS_URL
- AGENT_API_KEY
- LOG_LEVEL

## Screenshots
- [Deployment dashboard](screenshots/dashboard.png)
- [Service running](screenshots/running.png)
- [Test results](screenshots/test.png)
```

##  Pre-Submission Checklist

- [ ] Repository is public (or instructor has access)
- [ ] `MISSION_ANSWERS.md` completed with all exercises
- [ ] `DEPLOYMENT.md` has working public URL
- [ ] All source code in `app/` directory
- [ ] `README.md` has clear setup instructions
- [ ] No `.env` file committed (only `.env.example`)
- [ ] No hardcoded secrets in code
- [ ] Public URL is accessible and working
- [ ] Screenshots included in `screenshots/` folder
- [ ] Repository has clear commit history

---

##  Self-Test

Before submitting, verify your deployment:

```bash
# 1. Health check
curl https://your-app.railway.app/health

# 2. Authentication required
curl https://your-app.railway.app/ask
# Should return 401

# 3. With API key works
curl -H "X-API-Key: YOUR_KEY" https://your-app.railway.app/ask \
  -X POST -d '{"user_id":"test","question":"Hello"}'
# Should return 200

# 4. Rate limiting
for i in {1..15}; do 
  curl -H "X-API-Key: YOUR_KEY" https://your-app.railway.app/ask \
    -X POST -d '{"user_id":"test","question":"test"}'; 
done
# Should eventually return 429
```

---

##  Submission

**Submit your GitHub repository URL:**

```
https://github.com/your-username/day12-agent-deployment
```

**Deadline:** 17/4/2026

---

##  Quick Tips

1.  Test your public URL from a different device
2.  Make sure repository is public or instructor has access
3.  Include screenshots of working deployment
4.  Write clear commit messages
5.  Test all commands in DEPLOYMENT.md work
6.  No secrets in code or commit history

---

##  Need Help?

- Check [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- Review [CODE_LAB.md](CODE_LAB.md)
- Ask in office hours
- Post in discussion forum

---

**Good luck! **
