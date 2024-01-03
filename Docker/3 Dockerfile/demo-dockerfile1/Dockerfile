# 先指定一个基础镜像，然后在基础镜像上添加应用程序(基于层次结构来构建)

# node:14-alpine镜像是基于Linux发行版Alpine来构建的
FROM node:14-alpine

# copy source dest
# 源路径: 相对于Dockerfile文件的路径，目标路径: 相对于镜像的路径
COPY index.js /index.js

# 在镜像中运行应用程序
# CMD ["node", "/index.js"] 可执行程序的名称 可执行程序所接收到的参数
CMD node /index.js