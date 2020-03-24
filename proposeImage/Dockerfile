FROM python:3.6
COPY . /
RUN pip install -r /requirements.txt  -i https://pypi.tuna.tsinghua.edu.cn/simple
ENTRYPOINT ["python", "/propose.py"]
