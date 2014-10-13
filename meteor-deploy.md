# 미티어 배포

## 독립 서버에 배포하기

### 빌드  
소스가 있는 경로에서 다음 명령어를 실행한다:
```
$ meteor build <output-file-dir>
```

그러면 `output-file-dir` 디렉토리에 빌드 파일이 압축 파일의 형태로 저장된다.

### 압축 풀기  
해당 압축 파일을 푼다:
```
$ tar zxvpf filename.tar.gz
```

압축을 풀면 `bundle` 디렉토리가 만들어지고, 그 하위에 압축이 풀린다.  
`bundle` 디렉토리의 `README` 파일을 읽어보면, 설치 방법이 안내되어 있다.


### 설치 실행  
README 파일의 설치 안내를 보면, 3단계로 이루어진다:

1. `npm install` 실행  
2. 환경변수 `export`  
3. 앱 실행  

이 절차를 순서대로 진행한다.



* publish  
