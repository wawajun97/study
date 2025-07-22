# Nginx

## 배포 과정
1. app > app 이름 폴더 만들기
2. etc/nginx/sites-available 앱 이름 site 만들기
3. site 안에서 서버 설정
4. sites-available에 심볼릭 링크 만들기 (ex : sudo ln -s /etc/nginx/sites-available/[프젝명] /etc/nginx/sites-enabled/[프젝명])
5. nginx 재시작(sudo systemctl restart nginx)

> 심볼릭 링크 : 원본 파일과 동일하게 실행되도록 하는 링크. 윈도우에서 바로가기
