include .env.gke
export $(shell sed 's/=.*//' ./.env.gke)

create: set create-cluster connect
remove: set connect remove-cluster

load-test: set connect apply-load-config
fail-test: set connect apply-fail-config

create-cluster:
	gcloud beta container clusters create ${G_NAME} \
	--zone ${G_ZONE} \
	--username "admin" \
	--cluster-version "1.11.8-gke.6" \
	--machine-type "${G_MTYPE}" \
	--image-type "COS" \
	--disk-size "10" \
	--scopes "https://www.googleapis.com/auth/compute","https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" \
	--preemptible \
	--num-nodes "${G_NODES}" \
	--network "default" \
	--no-enable-cloud-logging \
	--enable-cloud-monitoring \
	--subnetwork "default" \
	--addons HorizontalPodAutoscaling,HttpLoadBalancing \
	--enable-autorepair

set:
	gcloud beta config set project ${G_PROJECTID}

connect:
	gcloud beta container clusters get-credentials ${G_NAME} --zone ${G_ZONE}

apply-load-config:
	sed -e 's=TARGET_ID_ADDR=$(TARGET_ID_ADDR)=g;s=LOAD_TEST_IMAGE=$(LOAD_TEST_IMAGE)=g;s=LOAD_TESTING_REPLICAS=$(LOAD_TESTING_REPLICAS)=g' ../templates/loadtesting-deployment.yaml > ./loadtesting-deployment.yaml
	kubectl apply -f ./loadtesting-deployment.yaml

apply-fail-config:
	sed -e 's=TARGET_ID_ADDR=$(TARGET_ID_ADDR)=g;s=FAIL_TEST_IMAGE=$(FAIL_TEST_IMAGE)=g;s=FAIL_TESTING_REPLICAS=$(FAIL_TESTING_REPLICAS)=g' ../templates/failuretesting-deployment.yaml > ./failuretesting-deployment.yaml
	kubectl apply -f ./failuretesting-deployment.yaml

remove-load-config:
	kubectl delete -f ./loadtesting-deployment.yaml

remove-fail-config:
	kubectl delete -f ./failuretesting-deployment.yaml

remove-cluster:
	gcloud container --project ${PROJECT_ID} clusters delete ${G_NAME} --zone ${G_ZONE} --async