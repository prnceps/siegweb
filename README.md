<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/kwon99/siegweb">
    <img src="https://github.com/kwon99/siegweb/blob/main/img/siege.png" alt="Logo" width="80" height="80">
  </a>

<h3 align="center">siegweb</h3>
</div>

## ๐จโ๐ป Introduction

CTFD ๊ธฐ๋ฐ์ ์น ๊ณต๋ฐฉ์  ์ค์  ํ๋ก๊ทธ๋จ

> ์๋ฒ์์ ์์ฑํ FLAG๋ฅผ ํด๋ผ์ด์ธํธ์ ์ ์ฉ, ๊ฐ ํด๋ผ์ด์ธํธ๋ฅผ ํดํนํ๋ฉฐ ์ ์๋ฅผ ์ป๋ ์์คํ์๋๋ค.

<img src="https://github.com/kwon99/siegweb/blob/main/img/ctfd1.png" width="1000">

<br /><br />

## โ๏ธ Setting

**ํด๋ผ์ด์ธํธ์ ๊ฒฝ์ฐ Docker ์ฌ์ฉ ์ ๋ฌด์ ๋ฐ๋ผ ๋๋ฉ๋๋ค.**

> Docker version์ ์ดํ์ ๋ด์ฉ๋ค์ด ๋ฏธ๋ฆฌ ์ธํ๋์ด์๋ Dockerfile์ ์ด์ฉํฉ๋๋ค.
> <br/>

**์๋ฒ์ ๊ฒฝ์ฐ ํน๋ณํ ๋ถ๊ธฐ์ ์ ์์ต๋๋ค.**

#### ์ค์น

```bash
git clone https://github.com/kwon99/siegweb.git /siegweb
```

<br /><br />

## ๐ป Client

> ์ฌ์ฉ๋๋ ๋จ ํ๋์ ํ์ผ (client.py)๋ ์๋ฒ๋ก๋ถํฐ FLAG๋ฅผ ๋ฐ์์ ํด๋ผ์ด์ธํธ์ ๋ฃ์ด์ฃผ๋ ์ญํ ์ ํฉ๋๋ค.

> ์ดํ์ ๋ชจ๋  ๋ช๋ น์ด๋ `sudo su` ์ํ์์ ์งํํ์์ผ ํธํฉ๋๋ค.

#### ์๊ฐ ์ค์ 

```bash
ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```

#### MySQL ๊ณ์  ๋ฐ FLAG DB ์์ฑ

> ์ด๋ฏธ ๊ณต๋ฐฉ์ ์ฉ ๊ณ์ ์ด ์กด์ฌํ๋ค๋ฉด ์๋ ๋ ์ค์ ์ญ์ ํ๊ณ  ์งํํฉ๋๋ค.

```sql
# client/client.sql
# https://github.com/kwon99/siegweb/blob/7cb5fa8cdaa1dce43f92c87f73dca50616b25424/client/client.sql#L2-L3
# user, passwd ๋ณ๊ฒฝ
CREATE USER 'user'@'localhost' IDENTIFIED BY 'passwd';
GRANT ALL PRIVILEGES ON *.* to 'user'@'localhost';
```

```sql
# sql ๋ก๊ทธ์ธ ์ดํ ์งํ (e.g. mysql -u root -p)
source /siegweb/client/client.sql
```

#### FLAG server ์์ฑ

```
mkdir /flag
cd /flag
touch flag
```

#### ๊ณ์  ๊ฐ ๋ณ๊ฒฝ

```python
# client/client.py
# https://github.com/kwon99/siegweb/blob/7cb5fa8cdaa1dce43f92c87f73dca50616b25424/client/client.py#L53-L54
# token, db ์ ๋ณด, ๊ณต๋ฐฉ์  ์๋ฒ ์ฃผ์ ๊ธฐ์
client = Client("token", "your_database_id", "your_database_pw", "server_address")
```

#### ์คํํ์ผ ๊ถํ ์ค์ 

```bash
chmod 600 /siegweb/client/client.py
```

#### Crontab ์ค์ 

```bash
crontab -e

# ๋ง์ง๋ง ์ค์ ์ถ๊ฐ
0 */3 * * * /usr/bin/python3 /siegweb/client/client.py
```

<br /><br />

## ๐ณ Client with DOCKER

> Docker๋ ์ ๋ด์ฉ๋ค์ ๋๋ถ๋ถ์ ์ค์ ํด๋ ์ํ์ด๋ฏ๋ก, ๋ณ๊ฒฝ ๋ถ๋ถ๋ง ๋ณ๊ฒฝํฉ๋๋ค.

#### MySQL ๊ณ์  ๋ฐ FLAG DB ์์ฑ

> ์ด๋ฏธ ๊ณต๋ฐฉ์ ์ฉ ๊ณ์ ์ด ์กด์ฌํ๋ค๋ฉด ์๋ ๋ ์ค์ ์ญ์ ํ๊ณ  ์งํํฉ๋๋ค.

```sql
# client/client.sql
# https://github.com/kwon99/siegweb/blob/7cb5fa8cdaa1dce43f92c87f73dca50616b25424/client/client.sql#L2-L3
# user, passwd ๋ณ๊ฒฝ
CREATE USER 'user'@'localhost' IDENTIFIED BY 'passwd';
GRANT ALL PRIVILEGES ON *.* to 'user'@'localhost';
```

#### ๊ณ์  ๊ฐ ๋ณ๊ฒฝ

```python
# client/client.py
# https://github.com/kwon99/siegweb/blob/7cb5fa8cdaa1dce43f92c87f73dca50616b25424/client/client.py#L53-L54
# token, db ์ ๋ณด, ๊ณต๋ฐฉ์  ์๋ฒ ์ฃผ์ ๊ธฐ์
client = Client("token", "your_database_id", "your_database_pw", "server_address")
```

<br /><br />

## ๐ฅ Server

> server.py๋ ๊ฐ ์ฐธ๊ฐ์ ๋ณ๋ก FLAG๋ฅผ ์์ฑํ๊ณ  ์ด๋ฅผ CTFD ์๋ฒ ๊ทธ๋ฆฌ๊ณ  JSON์ ์ ์ฅํฉ๋๋ค.

> JSON์ ์ฐธ๊ฐ์์ ์ ๋ณด์ ํ์ฌ ์๊ฐ์ FLAG๋ฅผ ์ ์ฅํฉ๋๋ค. ๋ณ๊ฒฝ๋  ๋ ๋ง๋ค `docker-compose restart` ํ์ํฉ๋๋ค.

> flask ์๋ฒ๋ ์ฐธ๊ฐ์์ ํ ํฐ์ ๋ง๋ FLAG๋ฅผ ๋ณด๋ด์ค๋๋ค.

#### ํ์ํ ํจํค์ง ์ค์น (python3, docker, docker-compose)

```bash
# ์คํ ๊ถํ ๋ถ์ฌ ๋ฐ ์คํ
sudo chmod +x /siegweb/server/requirement
sudo cd /siegweb/server && ./requirement
```

#### ์ ๋ณด ์๋ ฅ

```python
# https://github.com/kwon99/siegweb/blob/dfa1b264344c76cfcf273af214454041ea0fa161/server/json/user.json#L1-L4
# server/json/user.json
# ์ฐธ๊ฐ์ ํ๊ฒฝ์ ๋ง๊ฒ ๋ด์ฉ ๋ณ๊ฒฝ (token์ ์ฌ์ฉ์๋ฅผ ์ธ์ฆํ๋ ์ญํ ๋ก, ๋ฌด์์๋ก ์์ฑํ์๋ ๋๊ณ , ์ฐธ๊ฐ์๊ฐ ๋ง๋  ๊ฒ์ ๋ฐ์ผ์๋ ๋ฉ๋๋ค)
{
    "userA": [1, "Team A", "server_address", "token"],
    "userB": [2, "Team B", "server_address2", "token2"]
}
```

#### CTFD ๋ฐ flask ์คํ

```
sudo cd /seigweb/server && docker-compose up -d
```

#### ctfd ์ธํ

_์๋ฒ์ ์ ์ํด ๋ํ ์ธํ ๋ฐ ์ ์ ์ ํ ๋ฑ๋ก ํ์๋ฉด ๋ฉ๋๋ค_

#### DB ์ฐ๊ฒฐ

```bash
# ์ฌ๊ธฐ์ ๊ฑธ๋ฆฐ ์ปจํ์ด๋์ ์ด๋ฆ ๋ณต์ฌ (e.g. 7705656~01)
find /var/lib/docker/overlay2/ -name 'ctfd.db'
```

```python
# server/server.py
# https://github.com/kwon99/siegweb/blob/dfa1b264344c76cfcf273af214454041ea0fa161/server/server.py#L153
# your_container_name์ ์์์ ๋ณต์ฌํ ๋ด์ฉ์ผ๋ก ๋ณ๊ฒฝ
server = Server("your_container_name", delete)
```

```python
# server/server.py
# https://github.com/kwon99/siegweb/blob/dfa1b264344c76cfcf273af214454041ea0fa161/server/server.py#L8
# ubuntu(linux)๋ฅผ ๊ธฐ์ค์ผ๋ก ํํ๋ฆฟ์ ์ ์ํ์ผ๋ ๋ง์ฝ ๋ค๋ฅธ ํ๊ฒฝ์ผ ๊ฒฝ์ฐ ctfd.db์ ์์น๋ฅผ ์ฐพ์์ ์๋ ์ฝ๋์ path ๋ถ๋ถ์ ์์ ํด์ฃผ์๋ฉด ๋ฉ๋๋ค.
self.conn = sqlite3.connect(
    f"/var/lib/docker/overlay2/{path}/merged/opt/CTFd/CTFd/ctfd.db"
)
```

#### crontab ์ค์ 

```bash
crontab -e

# ๋ง์ง๋ง ์ค์ ์ถ๊ฐ
0 */3 * * * /usr/bin/python3 /siegweb/server/server.py
```

#### ์ค๋ฅ๊ฐ ๋  ๊ฒฝ์ฐ

> Challenge์ ๊ฐ์์ FLAG์ ๊ฐ์๊ฐ ๋ค๋ฅผ ๊ฒฝ์ฐ ์ค๋ฅ๊ฐ ๋ฐ์ํ  ์ ์์ต๋๋ค. (e.g. Challenge๋ง ์๊ณ  FLAG๊ฐ ์์ ๋)

```bash
# ๊ทผ๋ณธ์ ์ธ ์ด์ ๋ฅผ ์ฐพ์ ์ ๊ฑฐํ๋ ๊ฒ์ด ์ ์ผ ์ข์ ๋ฐฉ๋ฒ์ด์ง๋ง, ์ฌ์์น ์์ ๊ฒฝ์ฐ๋ฅผ ์์ ํด `--delete` ์ธ์๋ฅผ ์ถ๊ฐํ์์ต๋๋ค.
# --delete ์ธ์๋ฅผ ์ฌ์ฉํ๋ฉด ํ์ฌ CTFD์ ๋ชจ๋  Challenge์ FLAG๋ฅผ ์ด๊ธฐํํฉ๋๋ค.
python3 server.py --delete
```
