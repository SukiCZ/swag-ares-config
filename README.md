
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

## Modify generated code
```shell
find python/test/ -type f -exec sed -i 's/048012/CZ/g' {} +

# Fix min length for `kod_statu`
find python/swag_ares -type f -exec sed -i 's/kod_statu: Optional\[Annotated\[str, Field(min_length=3, /kod_statu: Optional\[Annotated\[str, Field(min_length=2, /g' {} +
# Fix min length for `ico`
find python/swag_ares -type f -exec sed -i 's/ico: Optional\[Annotated\[str, Field(min_length=8, /ico: Optional\[Annotated\[str, Field(min_length=7, /g' {} +
# Fix min length for `statni_obcanstvi`
find python/swag_ares -type f -exec sed -i 's/statni_obcanstvi: Optional\[Annotated\[str, Field(min_length=3, /statni_obcanstvi: Optional\[Annotated\[str, Field(min_length=2, /g' {} +
# Fix min length for `pravni_forma`
find python/swag_ares -type f -exec sed -i 's/pravni_forma: Optional\[Annotated\[str, Field(min_length=3, /pravni_forma: Optional\[Annotated\[str, Field(min_length=2, /g' {} +
# Fix min length for `pravni_forma` list
find python/swag_ares -type f -exec sed -i 's/pravni_forma: Optional\[List\[Annotated\[str, Field(min_length=3, /pravni_forma: Optional\[List\[Annotated\[str, Field(min_length=2, /g' {} +
# Fix min length for `financni_urad` list
find python/swag_ares -type f -exec sed -i 's/financni_urad: Optional\[List\[Annotated\[str, Field(min_length=3, /financni_urad: Optional\[List\[Annotated\[str, Field(min_length=2, /g' {} +
# Fix min length for `hodnota`
find python/swag_ares/models/pravni_forma_vr.py -type f -exec sed -i 's/hodnota: Optional\[Annotated\[str, Field(min_length=3, /hodnota: Optional\[Annotated\[str, Field(min_length=2, /g' {} +
find python/test/ -type f -exec sed -i "s/hodnota='CZ'/hodnota='112'/g" {} +


# Fix regex validation for `ico`
find python/swag_ares -type f -exec sed -i 's/if not re.match(r"\^\\d{8}\$", value):/if not re.match(r"\^\\d{7,8}\$", value):/g' {} +
find python/swag_ares -type f -exec sed -i 's/raise ValueError(r"must validate the regular expression \/\^\\d{8}\$\/")/raise ValueError(r"must validate the regular expression \/\^\\d{7,8}\$\/")/g' {} +

# Fix regex validation for `pravni_forma`
find python/swag_ares -type f -exec sed -i 's/if not re.match(r"\^\\d{3}\$", value):/if not re.match(r"\^\\d{2,3}\$", value):/g' {} +
find python/swag_ares -type f -exec sed -i 's/raise ValueError(r"must validate the regular expression \/\^\\d{3}\$\/")/raise ValueError(r"must validate the regular expression \/\^\\d{2,3}\$\/")/g' {} +

# Modify test value for `ico`
find python/test/ -type f -exec sed -i 's/0480728801234567/04807288/g' {} +
# Modify test value for `pravni_forma`
find python/test/ -type f -exec sed -i "s/pravni_forma='CZ'/pravni_forma='112'/g" {} +
find python/test/ -type f -exec sed -i "s/pravni_forma = 'CZ'/pravni_forma = '112'/g" {} +

# Fix unit test for `pravni_forma`
find python/test/ -type f -exec sed -i "s/self.assertEqual(self.inst_req_and_optional.pravni_forma, 'CZ')/self.assertEqual(self.inst_req_and_optional.pravni_forma, '112')/g" {} +

# Modify test value for `financni_urad`
find python/test/ -type f -exec sed -i "s/financni_urad='CZ'/financni_urad='060'/g" {} +
# Fix unit test for `financni_urad`
find python/test/ -type f -exec sed -i "s/self.assertEqual(self.inst_req_and_optional.financni_urad, 'CZ')/self.assertEqual(self.inst_req_and_optional.financni_urad, '060')/g" {} +

# Modify test value for `institucionalni_sektor2010`
find python/test/ -type f -exec sed -i "s/institucionalni_sektor2010='0480701234'/institucionalni_sektor2010='11002'/g" {} +
find python/test/ -type f -exec sed -i "s/institucionalni_sektor2010 = '0480701234'/institucionalni_sektor2010 = '11002'/g" {} +


```


## Publish to PyPi
```shell
# Publish to PyPi
cd python
pip install -U build twine
python -m build
python -m twine upload dist/*  # tip: check ~/.pypirc for credentials
```
