
# Swag ARES

Collection of resources to generate a client for [ARES](https://ares.gov.cz/swagger-ui/) API using OpenAPI generator.


## Generate client
```shell
# Generate the client
docker run --rm -v "${PWD}:/local" openapitools/openapi-generator-cli generate \
    -i https://ares.gov.cz/ekonomicke-subjekty-v-be/rest/v3/api-docs \
    -g python \
    -o local/python \
    -c local/config.yaml \
    -t local/templates
```

## Publish to PyPi
```shell
# Publish to PyPi
cd python
pip install -U build twine
python -m build
python -m twine upload dist/*  # tip: check ~/.pypirc for credentials
```
