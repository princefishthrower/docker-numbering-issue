There is an issue with docker's `scale` command. See as follows:

First, start up an instance of `app`:

```shell
docker-compose build --no-cache && docker-compose up -d
```

We get one named container `docker-numbering-issue_app_1`

Then, scale it up to 2 instances:

```shell
docker-compose up -d --scale app=2 --no-recreate
```

all is well, we get another named container `docker-numbering-issue_app_2`

But, if we remove `docker-numbering-issue_app_1` and then rename `docker-numbering-issue_app_2` to `docker-numbering-issue_app_1`:

```shell
docker-compose rm -f app_1 && docker rename docker-numbering-issue_app_2 docker-numbering-issue_app_1
```

and we try to scale again:

```shell
docker-compose up -d --scale app=2 --no-recreate
```

we'll see the new container is named `docker-numbering-issue_app_3`!!! Unexpected name generation, IMO. This could be expected behavior but I missed or couldn't find in the docs where this is explained.