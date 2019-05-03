##

> data lab에서 연습한 코드를 패키징 해서 Cloud ML Engine에 배포하여  scale out하는 예시 

##

gsutil
~~~bash
gsutil -m defacl ch -u $SVC_ACCOUNT:R gs://$BUCKET
gsutil -m acl ch -u $SVC_ACCOUNT:R -r gs://$BUCKET #?
gsutil -m acl ch -u $SVC_ACCOUNT:W gs://$BUCKET
~~~

gcloud
~~~bash
gcloud auth print-access-token
~~~

https://8081-dot-4869646-dot-devshell.appspot.com/notebooks/datalab/training-data-analyst/courses/machine_learning/deepdive/03_tensorflow/e_cloudmle.ipynb


https://github.com/GoogleCloudPlatform/training-data-analyst


Launching into Machine Learning

I compleated 3/5 of the specilization. The below is remained.

Course 4 - Feature Engineering
Course 5 - Art and Science of Machine Learning. 
