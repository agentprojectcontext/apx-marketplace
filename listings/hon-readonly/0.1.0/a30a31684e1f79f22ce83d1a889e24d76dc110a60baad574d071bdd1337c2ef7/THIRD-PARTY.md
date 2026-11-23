# Third-party dependencies

This package imports, but does not vendor, pyhOn 0.17.5 and MCP Python 1.30.0. uv installs their original distributions and locked transitive dependencies separately. Their license files and notices remain in those distributions.

- pyhOn: MIT, copyright (c) 2023 Andre Basche. Source and license: https://github.com/Andre0512/pyhOn (tag v0.17.5 / distribution 0.17.5).
- MCP Python SDK: MIT. Source and license: https://github.com/modelcontextprotocol/python-sdk (distribution 1.30.0).
- All resolved versions, artifact hashes and dependency relationships are in server.py.lock. uv is an external prerequisite, not redistributed here: https://github.com/astral-sh/uv.

The connector uses pyhOn's authentication and attribute reads instead of copying its code or implementing the cloud login independently. It intentionally bypasses pyhOn's higher-level appliance/MQTT control layer. No upstream license grants endorsement by the appliance manufacturers.
