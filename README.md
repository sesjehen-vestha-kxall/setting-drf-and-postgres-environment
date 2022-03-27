# setting-drf-and-postgres-environment

## requirements.txt

```text
django==<version>
djangorestframework==<version>
psycopg2==<version>
django-cors-headers==<version>
```

## If you have problems with psycopg2
### Fedora 34 / Red Hat / Centos 9
```shell
sudo dnf install libpq-devel python3-devel -vy
```
### Ubuntu
```shell
sudo apt-get install libpq-devel python3-devel -vy
```

## Solving database connection problems

change the peer authentication method, to `md5` inside 
`pg_hba.conf` which could be located at `/var/lib/pgsql/data/pg_hba.conf` 
or look up using `find / -name pg_hba.conf`.

```conf
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     <HERE>
# IPv4 local connections:
host    all             all             127.0.0.1/32            <HERE>
# IPv6 local connections:
host    all             all             ::1/128                 ident
```

Have doubts? Check this reference: https://docs.fedoraproject.org/en-US/quick-docs/postgresql/#pg_hba.conf

## DB Connection Configuration

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': '<db-name>',
        'USER': '<your-user-name>',
        'PASSWORD': '<password>',
        'HOST': '<default: 127.0.0.1>'
    }
}
```

## CORS Settings
```python
INSTALLED_APPS = [
    ...,
    "corsheaders",
    ...,
]
```

```python
MIDDLEWARE = [
    ...,
    "corsheaders.middleware.CorsMiddleware",
    "django.middleware.common.CommonMiddleware",
    ...,
]
```
