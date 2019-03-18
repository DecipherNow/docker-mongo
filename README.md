# docker-mongo

An OpenShift comptatable Docker image for a Mongo 4.X

## Running

To run this image:

        docker run -p 27017:27017 decipher/mongo:latest

If you need data to persist after the conainer exits you can mount a volume:

        docker run -p 27017:27017 -v ${PWD}/mongodb:/var/lib/mongodb/data decipher/mongo:latest

Lastly, it is not required but we recommend you run the container as an arbitrary user:

        docker run -p 27017:27017 -v ${PWD}/monogdb:/var/lib/mongodb/data -u 1024 decipher/mongo:latest

## Using

Provide the following environment variables when running to create an initial root user and database

        MONGO_INITDB_ROOT_USERNAME
        MONGO_INITDB_ROOT_PASSWORD
        MONGO_INITDB_DATABASE

To create a new database on launch, add a mongo file to `/docker-entrypoint-initdb.d/`

Here is an example file

    db.auth('mongo', 'mongo')
    db = db.getSiblingDB('data')
    db.createUser({
        user: 'mongo',
        pwd: 'mongo',
        roles: [
        {
            role: 'root',
            db: 'admin',
        },
        ],
    });

## Building

To build a new image from this repository:

        ./build.sh

This will build and tag the version of Nexus defined in the [VERSION](VERSION) file. Alternatively, you can build a specific version of Nexus by providing a version number:

        ./build.sh 3.15.0

## Publishing

To publish the image built by this repository:

        ./publish.sh

This will publish the tags corresponding to the version defined in the [VERSION](VERSION) file. Alternatively, you can publish a specific tags by providing a version number:

        ./publish.sh 3.15.0

## Contributing

1. Fork it
1. Create your feature branch (`git checkout -b my-new-feature`)
1. Commit your changes (`git commit -am 'Add some feature'`)
1. Push to the branch (`git push origin my-new-feature`)
1. Create new Pull Request
