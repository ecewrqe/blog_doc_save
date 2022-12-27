``` python
import logging

logging.basicConfig(filename="example.log",
                    filemode="w",
                    format="%(asctime)s,%(levelname)s,%(name)s,%(funcName)s,%(pathname)s,%(filename)s,%(message)s",
                    datefmt="%Y-%m-%d %H:%M:%S",
                    level=logging.INFO,
                    )   #����������ļ�
def func1():
    '''�˺�����ӡ��־'''
    logging.info("user logging ��")
    logging.warning("user logging warning")
    logging.critical("user logging critical")
func1()
```
##### log����(level)
```
CRITICAL=FATAL=50(Ĭ�������صļ���)
ERROR=40
WARNING=30
INFO=20
DEBUG=10(����������Ҫ���������)
NOTSET=0
```

### logging�µļ����ࣺ
##### logger:
logging.getLogger(logname->str)->loggerObj
logger.setLevel(level)
logger.addHandler(hdlr->handlerObj)
logger.hasHandler()->bool  ----�Ƿ���������
logger.error(msg,...)   -----��logger�Ӿ����ʽ�����
##### Formatter��ʽ:
logging.Formatter(fmt->str,datafmt->str)->formatterObj
logging��ʽ��
```
	%(<>)s          �ο�string��format
    %(name)s           logger����root
    %(levelno)s         ��־������ֵ
    %(levelname)s       ��־��������
    %(pathname)s        �ű��ļ�·��
    %(filename)s        �ű��ļ���
    %(module)s          Module (name portion of filename)
    %(lineno)d          ��ӡ����־���к�
    %(funcName)s       ��ӡ
    %(created)f         Time when the LogRecord was created (time.time()
                        return value)
    %(asctime)s           ��ʽʱ�䣬��������datafmt����
    %(msecs)d           Millisecond portion of the creation time
    %(relativeCreated)d Time in milliseconds when the LogRecord was created,
                        relative to the time the logging module was loaded
                        (typically at application startup time)
    %(thread)d          �߳�ID
    %(threadName)s      �߳���
    %(process)d         ����ID
    %(message)s         �����Ϣ
```
datafmt�ο�ʱ���ʽ

##### logging.Handler
filehandler����streamhandler����handler����ǰ������ʵ�ֽӿ�

logging.filehandler(filename,mode(a),encoding)    ��ӡ���ļ�
logging.streamhandler()��ӡ����Ļ

handler.setLevel(level)     ----handlerÿ�������Լ��ļ�������
handler.setFormatter(fmt)    ----��Ӹ�ʽ


