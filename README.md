**Openvas Docker container**

This container is based on Centos 8 for FIPS-140-2 compliance. It is a self contained Openvas Scanner with web console on port 80.  
This image has been updated to use Greenbone Vulnerability Manager (GVM) version 20.08

The container uses supervisord to manage all of the required services. There are technical limitations (redis and postgres must be connected to via unixsocket), that require all services to be run in the same container.

The following services are run.
- postgres : Used for persistent storage of configuration, jobs and reports.
- gvmd: core backend api service. 
- ospd-openvas: connects to gvmd to get tasks to run, then launches 'openvas' instances to run the tasks.
- redis: used by 'openvas' for caching data.
- gsad: The web frontend service.

Rather than having supervisord start all of the services directly, a 'startup' service is used to bootstrap the container and start services in the correct order, ensuring that services are not started until dependencies are ready.

**Launch**

	docker run -d -p 80:80 --name openvas grafana/openvas

	http://<IP>/
	Default login / password: admin / admin

**Launch with a Volume**

	docker volume create openvas

	docker run -d -p 80:08 -v openvas:/var/lib/gvm --name openvas grafana/openvas

**Set Admin Password**

	docker run -d -p 80:80 -e OV_PASSWORD=iliketurtles --name openvas grafana/openvas

**Update NVT and feed data**

	Note: This process may take take some time. 

	docker run -d -p 80:80 -e OV_UPDATE=yes --name openvas grafana/openvas

**Attach to running**

	docker exec -it openvas bash



