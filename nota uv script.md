
"I just ran into this, I am building my own Docker image successfully on top of 5.0.0rc1 and uv installing packages there. But when I switched to 4.1.2rc1 I had to change the Dockerfile to install the packages just with pip instead of uv.
 The other thing that would help here is versioned docs, because you are seeing instructions that got introduced to the repo after 4.1.1. If you could see a 4.1.1 version of the docs it would say to just pip install - and then that, plus checking out the repo to the appropriate tag as @mistercrunch suggests, would do it.
 Versioned docs should be in the pipeline with the Superset Improvement Proposal SIP-156: #32625"

Adjust the docker/docker-bootstrap.sh because it installs the packages inrequirements-local.txtvia uv.

```py
if [ -f "${REQUIREMENTS_LOCAL}" ]; then
  echo "Installing local overrides at ${REQUIREMENTS_LOCAL}"
  # I added this if/else so it uses pip if uv is not available
  if command -v uv > /dev/null 2>&1; then
    # Use uv in newer images
    uv pip install --no-cache-dir -r "${REQUIREMENTS_LOCAL}"
  else
    # Use pip in older images
    pip install --no-cache-dir -r "${REQUIREMENTS_LOCAL}"
  fi
else
  echo "Skipping local overrides"
fi
```
