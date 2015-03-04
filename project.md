Generic Project Guidelines
==========================

Projects MUST be versioned [Semantically][1]. Projects MUST declare a public API
in their documentation.

Projects MAY include public API documentation in `docs/`. When one is provided,
this documentation MUST be the canonical source of public code usage. This is
important when implementing Semantic Versioning and project support.

Projects MUST include a license, documented in a `LICENSE` file.

Projects SHOULD include a list of major contributors to the project included
in a `CONTRIBUTORS` file.

Projects SHOULD outline contribution guidelines, including a reference to this
and any other code standard in a `CONTRIBUTING` file.

Projects SHOULD include instructions for initial installation and setup
instructions in a `README` file.

Projects SHOULD include a [documented][2] list of major changes to the public
API in a `CHANGELOG` file.

Projects MAY include a list of other important package information including
more detailed copyright, license, and trademark information. If it does this
information SHOULD be kept in a `NOTICE` file.

[1]: http://semver.org/
[2]: http://keepachangelog.com/
