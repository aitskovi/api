api
===

This is a test for hosting a simple data api on github.com. YAML formatted data files sit in the \_data folder and are mapped to json endpoints. For example:

	_data/weight.yml -> /api/v1/weight.json

Running `rake publish` generates and publishes a jekyll site with the api off the latest master branch on github. The published site can be accessed at:

	<username>.github.com/api/v1/...
