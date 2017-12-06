
- Only user `amax` is allowed to use `sudo` on server 84. If need to use, you can
  - post a new issue here.
  - contact chenming or luzai, to access `sudo` from `amax`.
- If need to download config files, contact luzai, he will
  - distribute an `id_rsa` file so that you can access this private repo.
  - or add you to this repo as one of contributors.
- Feel free to issue and fix errors, including typos!

## Connect to Internet

Here provide a method to share Internet of your laptop to server.

### Prerequisite:

- openssh-client
  - windows system can try:
    - cygwin and install openssh-client
    - linux subsystem and install openssh-client
    - [Potentially work] some gui manipulations in putty, I do not find the way. Tell me if you are familiar with putty.

### Forward Dynamic Port

In the most case, this works:

``` bash
ssh -gfNTD 1080 username@serverip
proxychains curl ip.gs
```

If 1080 is being used, you can

- use existed 1080, and of course, other user's Internet. In the most time, luzai will share his Internet to server.
- Start a new port, as shown below

``` bash
cp /etc/proxychains.conf ./
vim ./proxychains.conf # and modify 1080 to 1081
ssh -gfNTD 1081 username@serverip
proxychains curl ip.gs
```

### Post Work

For convenience, you can add this to `.bashrc` or other `rc` files you are using.

``` bash
alias git='proxychains git'
alias pip='proxychains pip2'
alias pip2='proxychains pip2'
alias pip3='proxychains pip3'
alias wget='proxychains wget -c '
alias conda='proxychains conda'
```

## Be careful to use tensorflow!

``` python
def init_dev(n=(0,)):
    logging.info('use gpu {}'.format(n))
    from os.path import expanduser
    home = expanduser("~")
    if isinstance(n, int):
        n = (n,)
    devs = ''
    for n_ in n:
        devs += str(n_) + ','
    os.environ["CUDA_VISIBLE_DEVICES"] = devs
    os.environ['PATH'] = home + '/cuda-8.0/bin:' + os.environ['PATH']
    # os.environ['PATH'] = home + '/anaconda2/bin:' + os.environ['PATH']
    os.environ['PATH'] = home + '/usr/local/cuda-8.0/bin:' + os.environ['PATH']

    os.environ['LD_LIBRARY_PATH'] = home + '/cuda-8.0/lib64'
    os.environ['LD_LIBRARY_PATH'] = '/usr/local/cuda-8.0/lib64'
    # os.environ['PYTHONWARNINGS'] = "ignore"


def set_env(key, value):
    value = os.path.abspath(value)
    os.environ[key] = value + ':' + os.environ[key]


def allow_growth_tf():
    import tensorflow as tf
    _sess_config = tf.ConfigProto(allow_soft_placement=True)
    _sess_config.gpu_options.allow_growth = True
    return _sess_config


def allow_growth_keras():
    import tensorflow as tf
    tf_graph = tf.get_default_graph()
    _sess_config = allow_growth_tf()
    sess = tf.Session(config=_sess_config, graph=tf_graph)
    import keras.backend as K
    K.set_session(sess)
    return sess
```

## Other config files

copy them to your home path, i.e., ~.

- `.condarc`: use ustc's mirror
- `.pip`: use tsinghua's mirror
- `sources.list.14.04`: use aliyun's mirror -- has been put to /etc/apt/
- `proxychains.conf`: proxy for http(s) and sock5
- `tsocks.conf`: proxy for sock5
