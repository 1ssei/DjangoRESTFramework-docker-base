docker-compose up -> you can see api in localhost:8000

## local development
1. change setting.py replace ***** to actual data
2. docker-compose up

## Component Test +Coverage
test
1. you can test クラス単位もファイル単位もフォルダ単位も
docker exec -it drf_web_1 python application/manage.py test users.tests.UserInterestedCategoryTests
(foldername.filename.classname)

3. 終わってコミットする前には全体のCoverage見てみる
docker exec -it drf_web_1 /bin/bash -c "cd api && python manage.py test"
docker exec -it drf_web_1 coverage html
4. you can check test result in htmlcov/index.html

## Performance TEST
1.  http://localhost:8000/silk/ ここでログの分析ができる。処理時間長いRequestとか
2. 処理はずっとたまるのでリセットするときは　以下を実行
 docker exec -it drf_web_1 python application/manage.py  silk_clear_request_log

## Security TEST
docker exec -it drf_web_1 bandit -r api -f html -o test.html -x *test*.py
Open file test.html
+
https://observatory.mozilla.org/
## API Docs
1. http://localhost:8000/docs/# ここでAPIの確認とTestができる
2. class ViewSet(viewsets.ViewSet):
    """
        Only admin can use
    """ とかで説明を書くことができる
3. class Model(models.Model):
    test = models.ForeignKey(TEST, on_delete=models.CASCADE,help_text='test is test')
    Modelにhelp_textつけるとParameterの説明をかける
    
    docker stop $(docker ps -q)
    docker rm $(docker ps -q -a)
    docker rmi $(docker images -q)


## Develop HELP MEMO
1. how to implement Method Permisssion
mixins.ListModelMixin,
mixins.CreateModelMixin,
mixins.RetrieveModelMixin,
mixins.UpdateModelMixin,
mixins.DestroyModelMixin,
viewsets.GenericViewSet

or 
viewsets.ReadOnlyModelViewSet

2. status code
HTTP_200_OK
HTTP_201_CREATED
HTTP_202_ACCEPTED
HTTP_203_NON_AUTHORITATIVE_INFORMATION
HTTP_204_NO_CONTENT
HTTP_205_RESET_CONTENT
HTTP_206_PARTIAL_CONTENT
HTTP_207_MULTI_STATUS
HTTP_208_ALREADY_REPORTED
HTTP_226_IM_USED

HTTP_300_MULTIPLE_CHOICES
HTTP_301_MOVED_PERMANENTLY
HTTP_302_FOUND
HTTP_303_SEE_OTHER
HTTP_304_NOT_MODIFIED
HTTP_305_USE_PROXY
HTTP_306_RESERVED
HTTP_307_TEMPORARY_REDIRECT
HTTP_308_PERMANENT_REDIRECT

HTTP_400_BAD_REQUEST
HTTP_401_UNAUTHORIZED
HTTP_402_PAYMENT_REQUIRED
HTTP_403_FORBIDDEN
HTTP_404_NOT_FOUND
HTTP_405_METHOD_NOT_ALLOWED
HTTP_406_NOT_ACCEPTABLE
HTTP_407_PROXY_AUTHENTICATION_REQUIRED
HTTP_408_REQUEST_TIMEOUT
HTTP_409_CONFLICT
HTTP_410_GONE
HTTP_411_LENGTH_REQUIRED
HTTP_412_PRECONDITION_FAILED
HTTP_413_REQUEST_ENTITY_TOO_LARGE
HTTP_414_REQUEST_URI_TOO_LONG
HTTP_415_UNSUPPORTED_MEDIA_TYPE
HTTP_416_REQUESTED_RANGE_NOT_SATISFIABLE
HTTP_417_EXPECTATION_FAILED
HTTP_422_UNPROCESSABLE_ENTITY
HTTP_423_LOCKED
HTTP_424_FAILED_DEPENDENCY
HTTP_426_UPGRADE_REQUIRED
HTTP_428_PRECONDITION_REQUIRED
HTTP_429_TOO_MANY_REQUESTS
HTTP_431_REQUEST_HEADER_FIELDS_TOO_LARGE
HTTP_451_UNAVAILABLE_FOR_LEGAL_REASONS

HTTP_500_INTERNAL_SERVER_ERROR
HTTP_501_NOT_IMPLEMENTED
HTTP_502_BAD_GATEWAY
HTTP_503_SERVICE_UNAVAILABLE
HTTP_504_GATEWAY_TIMEOUT
HTTP_505_HTTP_VERSION_NOT_SUPPORTED
HTTP_506_VARIANT_ALSO_NEGOTIATES
HTTP_507_INSUFFICIENT_STORAGE
HTTP_508_LOOP_DETECTED
HTTP_509_BANDWIDTH_LIMIT_EXCEEDED
HTTP_510_NOT_EXTENDED
HTTP_511_NETWORK_AUTHENTICATION_REQUIRED