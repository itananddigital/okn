# okn
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
