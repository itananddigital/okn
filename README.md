

https://drive.google.com/file/d/1APp4nJiRI-kXebO7HDh1ewczHt8TUHxW/view?usp=sharing


https://drive.google.com/file/d/1Nig61qDFXTSefE0BtagcKm9_s-53QVbE/view?usp=sharing


https://drive.google.com/file/d/1wBYW7vIRYpQ9zk9CJHL4IqnfLAXsxFpO/view?usp=sharing



https://drive.google.com/file/d/1z9VLLsxVQSX_yKpvPYRTUv42vTFJUvWc/view?usp=sharing# okn
adsds
https://drive.google.com/file/d/1KwilYOkOWg9VAIN66a5g0j-gHDLqiVyH/view


pm2 start node_modules/next/dist/bin/next --name frontend -- start -p 3001


NEXT_PUBLIC_API_BASE_URL=http://192.168.6.168:3000


https://drive.google.com/file/d/1GtWx4Xz3LaCqeVRFrjJ4SkBKE77Mo-Bk/view?usp=sharing

https://drive.google.com/file/d/11yqy1fh4u8REpBhitiwIN9JWkU_-EoO7/view?usp=sharing



pip install --upgrade "flask-limiter<3"

pip install --upgrade "marshmallow>=3.20,<4"


nohup gunicorn -w 3 -k gevent -b 0.0.0.0:8088 "superset.app:create_app()" > ~/superset/superset.log 2>&1 &


ps aux | grep "superset.app:create_app()"
This will show lines like:

css
Copy code
supersetuser  1717  0.3  ...  gunicorn -w 3 -k gevent -b 0.0.0.0:8088 superset.app:create_app()
Note the PID (here 1717).

2. Stop the running Superset process
bash
Copy code
pkill -f "superset.app:create_app()"
Or, if you want to kill a specific PID:

bash
Copy code
kill -9 1717
3. Start Superset again (same as before)
bash
Copy code
nohup gunicorn -w 3 -k gevent -b 0.0.0.0:8088 "superset.app:create_app()" > ~/superset/superset.log 2>&1 &



It looks like your upgraded Superset instance can’t find the **pyodbc** driver:

```
DB engine Error
No module named 'pyodbc'
```

This typically happens when:

1. **pyodbc** Python package isn’t installed inside the same virtualenv/container where Superset runs.
2. System-level ODBC libraries (unixODBC + Microsoft SQL drivers) are missing or not linked.

---

### ✅ Fix Steps

#### 1. Install required system packages

Inside your Superset container/host:

```bash
sudo apt-get update && \
sudo apt-get install -y --no-install-recommends \
    build-essential \
    unixodbc-dev \
    unixodbc \
    curl \
    gnupg2
```

#### 2. Add Microsoft SQL Server ODBC driver (if you connect to MSSQL)

```bash
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
curl https://packages.microsoft.com/config/debian/11/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list
sudo apt-get update
ACCEPT_EULA=Y sudo apt-get install -y msodbcsql18 mssql-tools18
echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' | sudo tee -a /etc/bash.bashrc
```

#### 3. Activate Superset venv & install Python deps

If you’re running a **virtual environment**:

```bash
source /home/supersetuser/superset/superset-venv/bin/activate
pip install --no-cache-dir pyodbc SQLAlchemy pandas numpy
deactivate
```

If using **Docker**, add to your Dockerfile:

```dockerfile
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential unixodbc-dev unixodbc curl gnupg2 && \
    curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && \
    ACCEPT_EULA=Y apt-get install -y msodbcsql18 mssql-tools18 && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    pip install --no-cache-dir pyodbc SQLAlchemy pandas numpy
```

#### 4. Restart Superset

```bash
superset stop
superset start
# or systemctl/docker restart depending on setup
```

---

### 🔑 Tips

* Make sure the **pyodbc** installation is inside the same environment where `superset` binary lives (`which superset` will tell you).
* For Docker, rebuild and redeploy the container after editing the Dockerfile.
* If you’re connecting to MSSQL/Azure SQL, verify connection string uses the `mssql+pyodbc://` driver.

Once these packages are installed and the service restarted, Superset will be able to load the **pyodbc** engine and your dashboards should work again.





https://drive.google.com/file/d/1_skXcstb28UhblvHVXWFsDxPZNVGhSL6/view?usp=sharing

