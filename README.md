It was a bad idea. Locks != leader election :)

---

Idea:
- use (free-tier) AWS DynamoDB for leader election in an Elixir cluster
- leader accepts writes to local SQLite database (or some other resource, like Iceberg or Delta Lake log)
- followers implicitly redirect writes to the leader or respond with a header like `x-replay` or https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/307 (https://alanflavell.org.uk/www/post-redirect.html)
- leader publishes WAL changes to the followers (LifeFS / Litestream-style)
- leader incrementally backs up SQLite to S3 (Litestream-style)

More info:
- https://aws.amazon.com/blogs/database/building-distributed-locks-with-the-dynamodb-lock-client/
- https://github.com/awslabs/amazon-dynamodb-lock-client
- https://github.com/wolfeidau/dynalock
- https://github.com/intercom/lease
- https://github.com/benbjohnson/litestream
- https://github.com/superfly/litefs
- https://iceberg.apache.org/concepts/catalog/
- https://lakefs.io/blog/iceberg-catalog/
