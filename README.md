# AWS-ECS-test
aws docker container service test


# Use
## 0. 필수 설치 파일

### AWS CLI 설치
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

### 도커 설치
sudo yum install docker -y
sudo service docker start
sudo usermod -a -G docker ec2-user


## 1. 깃허브에서 도커파일, 실행파일 다운로드

git clone https://github.com/hyunku/AWS-ECS-test.git
cd AWS-ECS-test


## 2. 도커 이미지 생성

docker build -t 이미지명 .

## 3. ECR 환경설정

export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile


## 4. 이미지 태깅

docker tag "이미지명" $ACCOUNT_ID.dkr.ecr."ap-northeast-2".amazonaws.com/"리포지토리명"


## 5. ECR에 이미지 업로드

aws ecr get-login-password --region "ap-northeast-2" | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr."ap-northeast-2".amazonaws.com
docker push $ACCOUNT_ID.dkr.ecr."ap-northeast-2".amazonaws.com/"리포지토리명"
