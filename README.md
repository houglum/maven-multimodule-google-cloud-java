## Getting The Code

```shell
git clone https://github.com/houglum/maven-multimodule-google-cloud-java
cd maven-multimodule-google-cloud-java
git submodule update --init --recursive
```

## Building/Running

These instructions use [Maven](https://maven.apache.org/) to build and run the
project.

**NOTE**: All `mvn` commands should be run from the root of the repository.

### Setting Up Credentials

The main module uses [Google Application Default Credentials](https://cloud.google.com/docs/authentication/production).

*  If you have a JSON keyfile, run this command so that the program knows where
   to find your keyfile:

   ```shell
   export GOOGLE_APPLICATION_CREDENTIALS="/path/to/keyfile.json"
   ```

*  If you're running on a GCE instance and would like to use GCE service account
   auth instead of a keyfile, make sure that the instance's default service
   account has been granted an IAM role that grants the
   `iam.serviceAccounts.signBlob` permission.

### Building The Project

To build all the modules and avoid running `google-cloud-java` tests, run:

```shell
mvn install -DskipTests
```

If you'd like to rebuild the project after altering the main module, it is much
faster to only recompile that module. This can be accomplished using maven's
`--projects` flag, as shown below:

```shell
mvn install -DskipTests --projects main-module
```

Similarly, if you edit both the google-cloud-storage and main-module projects,
you can supply both, delimited by a comma:

```shell
mvn install \
    -DskipTests \
    --projects main-module,google-cloud-java/google-cloud-clients/google-cloud-storage
```

### Running The Project

You'll need to supply your resource names used for generating signed URLs:

```shell
BKT_NAME="your-bucket-name-here"
GET_OBJ_NAME="name-of-an-object-in-your-bucket-that-already-exists"

# This is the name of the object you want to allow callers to upload bytes for.
# Thus, this object doesn't need to already exist.
PUT_OBJ_NAME="name-of-an-object-you-want-to-create"
```

After you've built the project, you can invoke this command to run it:

```shell
mvn exec:java --projects main-module \
    -Dexec.args="${BKT_NAME:?must be set} ${GET_OBJ_NAME:?must be set} ${PUT_OBJ_NAME:?must be set}"
```
