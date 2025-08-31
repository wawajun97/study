## 주요 용어
### Workflow
  - 하나 이상의 작업을 실행할 구성 가능한 자동화된 프로세스
  - YAML 파일로 작성되고 Event로 트리거될 때의 동작을 정의
  
### Event
  - Workflow 실행을 트리거하는 특정 활동
    - 특정 브랜치에 push
    - 특정 시간

### Job 
  - 실행가능한 가장 작은 작업 단위
  - 여러개의 Step으로 구성
### Step
  - Job 내부에서 순차적으로 실행되는 명령
  - 미리 정의된 action으로 구성

### Action
  - Github Actions에서 실행 가능한 재사용 가능한 작업단위
  - 커스텀 액션을 작성하거나 마켓 플레이스에서 다운로드 가능

### 에러

아래와 같이 작성 시 war가 아닌 build 디렉토리가 복사됨
 
 ```       
    - name: Copy WAR to EC2
      uses: appleboy/scp-action@v1
      with:
        host: ${{ secrets.HEYLOCAL_HOST }}
        username: ${{ secrets.HEYLOCAL_USERNAME }}
        key: ${{ secrets.HEYLOCAL_PRIVATEKEY }}
        port: 22
        source: ./build/libs/heylocal-server-0.0.1-SNAPSHOT.war
        target: /opt/temp/
 ```

해결 : war 파일을 현위치에 복사 후 복사된 war를 서버에 복사

  ```
      - name: Change filename
        run: mv ./build/libs/heylocal-server-0.0.1-SNAPSHOT.war ./heylocal.war
        
      - name: Copy WAR to EC2
        uses: appleboy/scp-action@v1
        with:
          host: ${{ secrets.HEYLOCAL_HOST }}
          username: ${{ secrets.HEYLOCAL_USERNAME }}
          key: ${{ secrets.HEYLOCAL_PRIVATEKEY }}
          port: 22
          source: heylocal.war
          target: /opt/temp/
  ```

