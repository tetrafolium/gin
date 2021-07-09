FROM python:3-alpine AS linter-stage

### Install yamllint tool ...
RUN pip3 install --no-cache-dir 'yamllint>=1.24.0,<1.25.0' && \
    echo "+++ $(yamllint --version)"

ENV SRCDIR="${HOME}/src" \
    REPOPATH="github.com/tetrafolium/gin"
ENV REPODIR="${SRCDIR}/${REPOPATH}"

ARG OUTDIR
ENV OUTDIR="${OUTDIR:-"/.reports"}"

RUN mkdir -p "${REPODIR}" "${OUTDIR}"
COPY . "${REPODIR}"
WORKDIR "${REPODIR}"

### Run yamllint ...
RUN yamllint -f parsable . > "${OUTDIR}/yamllint.issues" || true
RUN ls -la "${OUTDIR}"
