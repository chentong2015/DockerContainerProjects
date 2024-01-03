FROM node:18-alpine

# 定义容器在运行时的root目录
# 通过docker exec进入容器，执行自定义操作以及自动化命令
WORKDIR /app

# 定义要copy到镜像中的文件
COPY . .

COPY package.json yarn.lock ./
RUN yarn install --production

# 定义在容器运行时要执行的程序/命令
CMD ["node", "src/index.js"]

# 定义container port, 通过该端口对外暴露访问
EXPOSE 3000