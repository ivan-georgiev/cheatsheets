# Python

## Files

```python
import json
from io import StringIO


def main():

    filename = 'file.txt'
    jfilename = 'file.json'

    # read line by line
    with open(filename, 'r') as f:
        for line in f:
            # print(line)
            pass

    # read all lines
    with open(filename, 'r') as f:
        allLines = f.read()
        # print(allLines)
        pass

    # read binary
    chunckSize = 1024
    with open(filename, 'rb') as f:
        byte = f.read(chunckSize)
        while byte:
            byte = f.read(chunckSize)
            # print(byte)
            pass

    # read binary in 3.8
    with open(filename, 'rb') as f:
        while (byte := f.read(chunckSize)):
            print(byte)
            pass

    # read as tuple and strip
    with open(filename, 'r') as f:
        allLines = tuple(map(str.strip, f))
        # print(allLines)

    # read as json
    with open(jfilename, 'r') as f:
        jf = json.load(f)
        # print(jf)

    # write - fail if exists
    try:
        with open('wfile1.txt', 'x', encoding='utf-8') as f:
            f.write('line')
    except FileExistsError as e:
        print(str(e))

    # write - append, w: to create new
    with open('wfile1.txt', 'a', encoding='utf-8') as f:
        f.write('line')

    # fake file
    with StringIO() as f:
        f.write('write line ')
        print('print line', file=f)
        print(f.getvalue())
        f.seek(0)
        print(f.readlines())


if __name__ == '__main__':
    main()

```

## OS Exec

```python
from pathlib import Path
import subprocess
import os


def main():
    filestuff()
    processstuff()


def filestuff():

    testdirname = 'workdata'

    # current dir
    cDir = Path(__file__).resolve().parent

    # build path
    testdir = cDir.joinpath(testdirname)

    # create dir
    Path(testdir.joinpath('mydir')).mkdir(parents=True, exist_ok=True)

    # creare file in dir and rename it
    Path(testdir.joinpath('editorconfig')).write_text('# config goes here')

    # rename a file
    try:
        Path(testdir.joinpath('editorconfig')).rename(
            testdir.joinpath('editorconfig.ini'))
    except FileExistsError as e:
        pass

    # force rename a file
    Path(testdir.joinpath('editorconfig')).replace(
        testdir.joinpath('editorconfig.ini'))

    # search dir
    toplevel = list(Path.cwd().glob('*.py'))

    # serach tree
    treelevel = list(Path.cwd().glob('**/*.ini'))
    treelevel2 = list(Path.cwd().rglob('*.ini'))

    # list of child directories
    cDirChildDirs = [x for x in cDir.iterdir() if x.is_dir()]


def processstuff():

    r = subprocess.run(['ping', '8.8.8.8'],
                       stdout=subprocess.PIPE,
                       stderr=subprocess.PIPE
                       )
    print(r.returncode, r.stdout.decode('utf=8'), r.stderr.decode('utf-8'))

    r = subprocess.run(['ping', '-ivalid'],
                       stdout=subprocess.PIPE,
                       stderr=subprocess.STDOUT
                       )
    print(r.returncode, r.stdout.decode('utf=8'))

    r = subprocess.run(['ping', '8.8.8.8'],
                       capture_output=True
                       )
    print(r.returncode, r.stdout.decode('utf=8'), r.stderr.decode('utf-8'))

    #run(..., check=True, stdout=PIPE).stdout
    r = subprocess.check_output(
        ['ping', '8.8.8.8']).decode('utf-8').strip().splitlines()
    print(r)

    # all params
    r = subprocess.run(['pwsh', '-c', '"pwd"'],
                       capture_output=True,
                       shell=False,
                       cwd=None,
                       timeout=None,
                       check=False,
                       env=os.environ.copy())
    print(r.returncode, r.stdout.decode('utf=8'), r.stderr.decode('utf-8'))

    # async and wait
    r = subprocess.Popen(['ping', '8.8.8.8'],
                         stdout=subprocess.PIPE,
                         stderr=subprocess.PIPE,
                         shell=False,
                         cwd=None,
                         env=os.environ.copy())
    outs, errs = r.communicate(timeout=10)
    print(r.returncode, outs.decode('utf=8'), outs.decode('utf-8'))


if __name__ == '__main__':
    main()

```

## Web Requests

```python
import requests
import mimetypes
import pathlib


def main():

    mimetypes.init()

    url = 'https://jsonmock.hackerrank.com/api/football_matches?year=2011'

    kwargs = {
        'method': 'GET',
        'url': url,
        'headers': {
            'Content-Type': 'application/json',
            'Accept': 'application/json;version=1'
        },
        'verify': True,
        'allow_redirects': False
    }
    resp = requests.request(**kwargs)

    resp.raise_for_status()

    print(resp.json())
    print(resp.headers)
    print(resp.cookies)
    print(resp.text)
    print(resp.content)
    print(resp.status_code)

    # save to disk
    with open('savedApi.json', 'wb') as f:
        for chunk in resp.iter_content(chunk_size=128):
            f.write(chunk)

    # POST
    # payload = {'some': 'data'}

    # send form-encoded
    # resp = requests.post(url, data=payload)

    # send non-form encoded
    # resp = requests.post(url, data=json.dumps(payload))
    # resp = requests.post(url, json=payload)

    fname = 'savedApi.json'
    files = {'file': (fname,
                      open(fname, 'rb'),
                      mimetypes.types_map[pathlib.Path(
                          fname).suffix],
                      {'Expires': '0'})
             }
    #resp = requests.post(url, files=files)


if __name__ == '__main__':
    main()

```

## Input

TODO

## Collections

TODO

## Decorators

TODO

## OOP

TODO

## Flask

TODO