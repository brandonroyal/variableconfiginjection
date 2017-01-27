# Variable Config Injection
Need to leverage Docker environment variables in your .NET Framework projects without code changes?  Config Inject is here to help.  This is a simple PowerShell tool you can drop in your Dockerfile to take advantage of environment variables in your project config files. While the example below are for .NET application, this tool can be used for any project with plain text config files.

## Usage
1) Modify config file, replacing any text value you want replaced with environment variables using double brackets `{{}}`
```
...
<appSettings>
    <add key="Foo" value="{{FOO}}"/>
    <add key="Bar" value="{{BAR}}">
</appSettings>
...
<connectionStrings>
    <add name="DefaultConnection" connectionString="Server={{SQL_HOSTNAME}};Database={{DB_NAME}};Trusted_Connection=True;" />
</connectionStrings>
...
```


2) Download the tool into your Dockerfile using the `ADD` and `CMD` directives
```
...
ADD https://raw.githubusercontent.com/BrandonRoyal/variableconfiginjection/master/Inject-Variables.ps1 /
...
CMD /Inject-Variables.ps1 -ConfigPath ./Web.config; <command_to_start_your_app>
...
```

3) Pass environment variables into a `docker run` command, a `docker service create` command or a `docker-compose.yml` file

### docker run
```
docker run -e FOO=bar -e BAR=baz -e SQL_HOSTNAME=db.example.com -e DB_NAME=db -p 80:80 broyal/sample-iis:latest
```

### docker service create
```
docker service create -e FOO=bar -e BAR=baz -e SQL_HOSTNAME=db.example.com -e DB_NAME=db -p mode=host,target=80,published=80 broyal/sample-iis:latest
```

### docker-compose.yml
```
...
sample-iis:
    image: broyal/sample-iis:latest
    environment: 
        - FOO=bar
        - BAR=baz
        - SQL_HOSTNAME=db.example.com
        - DB_NAME=db
    ports:
        - 80:80
...
```

## Examples

* [IIS Example](/examples/IIS/)