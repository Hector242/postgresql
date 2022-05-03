# postgresql
data base postgresql for kubernetes

## Step 1: Creating a PostgreSQL secret
First, create a Kubernetes Secret for the PostgreSQL username and password

## Step 2: Creating a PostgreSQL persistent volume

PostgreSQL needs a persistent volume to store data; we'll create one along with a PersistentVolumeClaim. In this case, we're claiming the whole volume - but claims can ask for only part of a volume as well.

This file contains definitions for two different kinds, separated by a line with a triple dash. This syntax is helpful if you want to consolidate related Kubernetes definitions in a single file and apply them at the same time.

## Step 3: Creating a PostgreSQL deployment

Now we can create a Kubernetes Deployment descriptor for the PostgreSQL database deployment itself.

If you're not used to Kubernetes, this is a lot to take in. We're describing a Deployment (one or more instances of an application) that we'd like Kubernetes to know about in the metadata block.

The spec block describes the desired state. Here we've requested Kubernetes create 1 replica (running instance of PostgreSQL), and to create the replica with the given pod template, which again contains Kubernetes metadata and a desired state. The template spec shows one container, created from the published postgres:13.2-alpine Docker image.
Note the envFrom and secretRef - this tells Kubernetes to fill environment variables in the container with values from the Secret we created. We've also referenced the volume created for the deployment, and given it the mount path expected by PostgreSQL.

## Step 4: Creating a PostgreSQL service

The database pod is running, but how does another pod connect to it?

Kubernetes pods are transient - they can be killed, restarted, or created dynamically. Therefore we don't want to try to connect to pods directly, but rather create a Kubernetes Service. Services keep track of pods and direct traffic to the right place.
