# Sphinx (일명 : 스핑크스)

무료 오픈소스 검색엔진을 찾다가 적용한 sphinx

몇가지 명령어를 사용하면 정말 간단히 사용할 수 있습니다.

가장 중요한 것은 config 환경설정 파일입니다. 아래 예제를 보도록 하겠습니다.

```
source system_error
{
        type                    = mysql #db 종류를 넣어주면 됩니다.
        sql_host                = localhost #호스트입력 합시다.
        sql_user                = #유저정보를 넣읍시다
        sql_pass                = 
        sql_db                  = sphinx #db명을 넣어주면 됩니다.
        sql_port                = 3306 # optional, default is 3306
       #sql_sock                = /home/und3r/mysql/tmp/mysql.sock
          #->sock파일이 있으면 경로 확인해서 지정해주면 됩니다. 추후 socket통신 때문에 사용한다고 합니다.
        sql_query_pre = SET NAMES utf8 #한글설정을 위해서 입력합니다.
        sql_query               = SELECT sNo, error_code, error_status, error_desc FROM system_error 
          #-> 이 부분이 핵심입니다. 원하는 데이터를 인덱싱 하기위해 사용되어집니다. 
          #-> 기본적으로 첫 row는 프라이머리키로 int타입으로 들어와야합니다. 그래서 sno의 경우 자동으로 받아지는 형태입니다. 자세한 쓰임새는 잘 모르겠습니다. 아무튼 첫 row는 자동으로 받아지기 때문에 입력하지 않는다는 것을 인지하면 되겠습니다.
        #sql_attr_uint           = id
        sql_field_string        = error_status #각 필드에 불러온 데이터를 넣음
        sql_field_string        = error_code
        sql_field_string        = error_desc
        #sql_attr_timestamp     = date_added
}
#인덱스 설정입니다.
index system_error
{
	source		= system_error #위의 소스명과 동일하게 작성합니다.
	path		= C:/sphinx-3.1.1/bin/system_error #자신이 원하는 경로지정
	charset_table = 0..9, A..Z->a..z, _, a..z, U+410..U+42F->U+430..U+44F, U+430..U+44F #한글 지원시 필요합니다.
	ngram_len = 1
	#한글 검색시 필요
	ngram_chars =  U+4E00..U+9FBB, U+3400..U+4DB5, U+20000..U+2A6D6, U+FA0E, U+FA0F, U+FA11, U+FA13, U+FA14, U+FA1F, U+FA21, U+FA23, U+FA24, U+FA27, U+FA28, U+FA29, U+3105..U+312C, U+31A0..U+31B7, U+3041, U+3043, U+3045, U+3047, U+3049, U+304B, U+304D, U+304F, U+3051, U+3053, U+3055, U+3057, U+3059, U+305B, U+305D, U+305F, U+3061, U+3063, U+3066, U+3068, U+306A..U+306F, U+3072, U+3075, U+3078, U+307B, U+307E..U+3083, U+3085, U+3087, U+3089..U+308E, U+3090..U+3093, U+30A1, U+30A3, U+30A5, U+30A7, U+30A9, U+30AD, U+30AF, U+30B3, U+30B5, U+30BB, U+30BD, U+30BF, U+30C1, U+30C3, U+30C4, U+30C6, U+30CA, U+30CB, U+30CD, U+30CE, U+30DE, U+30DF, U+30E1, U+30E2, U+30E3, U+30E5, U+30E7, U+30EE, U+30F0..U+30F3, U+30F5, U+30F6, U+31F0, U+31F1, U+31F2, U+31F3, U+31F4, U+31F5, U+31F6, U+31F7, U+31F8, U+31F9, U+31FA, U+31FB, U+31FC, U+31FD, U+31FE, U+31FF, U+AC00..U+D7A3, U+1100..U+1159, U+1161..U+11A2, U+11A8..U+11F9, U+A000..U+A48C, U+A492..U+A4C6
} 
indexer
{
        mem_limit               = 128M
}
#search데몬파일 설정입니다. 아래 경로에 해당하는 것들은 자신이 원하는 위치로 모두 변경하시면 되겠습니다. 나머지는 그대로 사용하시면 될 거 같습니다.
searchd
{
        listen                  = 9312
        listen                  = 9306:mysql41
        log                     = C:/sphinx-3.1.1/bin/temp/searchd.log
        query_log               = C:/sphinx-3.1.1/bin/temp/query.log
        read_timeout            = 5
        max_children            = 30
        pid_file                = C:/sphinx-3.1.1/bin/searchd.pid
        seamless_rotate         = 1
        preopen_indexes         = 1
        unlink_old              = 1
        workers                 = threads # for RT to work
        binlog_path             = C:/sphinx-3.1.1/bin/binlog
}
```

설명에 나와있는 정도만 바꿔주고 설정하면 기본적인 부분은 사용하실 수 있습니다.

물론 추가적으로 더욱 상세한 설정을 자신의 입 맛에 맞게 바꾸려면 하나하나 옵션에 대해서 찾아보시고 추가하거나 변경하시면 될 거 같습니다.



이상으로 기본적인 sphinx 설정파일에 대한 설명을 마치겠습니다.