# ActiveData-redash-query-runner

Query Runner for Redash


## Instructions 

These instructions do not work

	git clone https://github.com/getredash/redash.git
	git clone https://github.com/klahnakoski/ActiveData-redash-query-runner.git
	
	chmod u+x redash/setup/ubuntu/bootstrap.sh
	sudo ./redash/setup/ubuntu/bootstrap.sh

	sudo supervisorctl stop redash_server
	sudo mkhomedir_helper redash
	
	sudo cp ~/ActiveData-redash-query-runner/active_data.py /opt/redash/current/redash/query_runner/
	sudo chown redash:nogroup /opt/redash/current/redash/query_runner/active_data.py

	sudo -i -u redash
	export REDASH_ADDITIONAL_QUERY_RUNNERS=redash.query_runner.active_data
	export REDASH_LOG_LEVEL=DEBUG
	cd /opt/redash/current
	/opt/redash/current/bin/run gunicorn -b 127.0.0.1:5000 --name redash -w 4 --max-requests 1000 redash.wsgi:app


## Testing

I do not know where the log files for Redash go (?`/var/log/supervisor`?), so stop the Supervisor service 

	sudo supervisorctl stop all
	sudo deluser redash
    sudo adduser --system --disabled-login --gecos "" redash
    sudo supervisorctl start all
	sudo supervisorctl stop redash_server
 
Then run the service directly from the `redash` command line:

	sudo -i -u redash
	export REDASH_ADDITIONAL_QUERY_RUNNERS=redash.query_runner.active_data
	export REDASH_LOG_LEVEL=DEBUG
	cd /opt/redash/current
	/opt/redash/current/bin/run gunicorn -b 127.0.0.1:5000 --name redash -w 4 --max-requests 1000 redash.wsgi:app
