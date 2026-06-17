# 部署指南

## 🚀 本地开发

### 环境要求
- 现代浏览器（Chrome, Firefox, Safari, Edge）
- 本地HTTP服务器

### 快速启动

**方案1: 使用Python**
```bash
# Python 3
python -m http.server 8000

# Python 2
python -m SimpleHTTPServer 8000
```
然后访问 `http://localhost:8000`

**方案2: 使用Node.js**
```bash
npm install
npm start
```

**方案3: 使用Live Server（推荐开发）**
```bash
npm install -D live-server
npx live-server
```

## 🌐 在线部署

### GitHub Pages 部署

1. **确保仓库设置启用Pages**
   - 进入仓库 Settings → Pages
   - 选择 Branch: main 
   - 选择 Folder: / (root)

2. **访问地址**
   ```
   https://Biuxiao.github.io/student/
   ```

3. **自定义域名（可选）**
   - 添加 CNAME 文件
   - 在DNS配置中指向GitHub Pages

### Netlify 部署

1. **连接GitHub仓库**
   - 登录 https://netlify.com
   - 选择 "New site from Git"
   - 选择该仓库

2. **部署设置**
   - 构建命令：留空
   - 发布目录：. (根目录)

3. **自动部署**
   - 每次push到main分支自动部署

### Vercel 部署

1. **导入项目**
   ```bash
   vercel
   ```

2. **设置环境**
   - 自动检测为静态站点
   - 默认设置即可

3. **访问**
   - 自动生成域名
   - 支持自定义域名

## 📦 Docker 部署

创建 `Dockerfile`:
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

构建和运行：
```bash
# 构建镜像
docker build -t student-platform .

# 运行容器
docker run -p 8080:80 student-platform

# 访问
http://localhost:8080
```

## 🔧 Web服务器配置

### Nginx 配置

```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/student;
    
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    # 缓存静态资源
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }
}
```

### Apache 配置

```apache
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot /var/www/student
    
    <Directory /var/www/student>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
        
        # 支持所有页面直接访问
        FallbackResource /index.html
    </Directory>
</VirtualHost>
```

## 📊 部署清单

- [ ] 测试所有页面链接
- [ ] 验证资源文件路径正确
- [ ] 检查浏览器兼容性
- [ ] 移除开发调试代码
- [ ] 优化图片和资源大小
- [ ] 配置HTTPS
- [ ] 设置缓存策略
- [ ] 监控错误日志

## 🔒 安全建议

1. **HTTPS** - 强制使用HTTPS
   ```nginx
   return 301 https://$server_name$request_uri;
   ```

2. **安全头**
   ```nginx
   add_header X-Frame-Options "SAMEORIGIN";
   add_header X-Content-Type-Options "nosniff";
   add_header X-XSS-Protection "1; mode=block";
   ```

3. **CORS** - 如需API集成
   ```javascript
   // 服务器端配置CORS
   header('Access-Control-Allow-Origin: *');
   ```

## 📈 性能优化

1. **启用Gzip压缩**
   ```nginx
   gzip on;
   gzip_types text/html text/css text/javascript application/javascript;
   ```

2. **CDN加速** - 使用Cloudflare等CDN服务

3. **资源优化**
   - 压缩CSS/JS
   - 优化图片格式
   - 使用懒加载

4. **缓存策略**
   ```nginx
   # HTML 不缓存
   location ~ \.html$ {
       add_header Cache-Control "no-cache, no-store, must-revalidate";
   }
   
   # 静态资源缓存
   location ~* \.(js|css|images)$ {
       expires 30d;
   }
   ```

## 🐛 故障排查

### 404 错误
- 检查文件路径是否正确
- 确保服务器配置支持目录重定向

### 资源加载失败
- 验证CSS/JS路径
- 检查浏览器控制台错误
- 确认服务器权限

### 功能不工作
- 检查jQuery是否加载
- 验证Axure脚本完整性
- 检查浏览器兼容性

## 📞 支持

遇到问题？
1. 检查 [GitHub Issues](https://github.com/Biuxiao/student/issues)
2. 查看浏览器开发者工具
3. 测试不同浏览器
4. 检查网络连接

## 📚 参考资源

- [GitHub Pages 文档](https://pages.github.com/)
- [Netlify 文档](https://docs.netlify.com/)
- [Nginx 文档](https://nginx.org/en/docs/)
- [Axure Player 文档](https://www.axure.com/)
