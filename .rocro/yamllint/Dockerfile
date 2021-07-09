FROM python:alpine AS linter-stage

### Install golang ...
RUN apk add --update --no-cache go=1.15.13 && \
    echo "+++ $(go version)"

ENV GOBIN="$GOROOT/bin" GOPATH="/.go"
ENV PATH="${GOPATH}/bin:/usr/local/go/bin:$PATH"

### Install yamllint tool ...
RUN pip3 install --no-cache-dir 'yamllint>=1.24.0,<1.25.0' && \
    echo "+++ $(yamllint --version)"

ENV REPOPATH="github.com/tetrafolium/gin" \
    TOOLPATH="github.com/tetrafolium/inspecode-tasks"
ENV REPODIR="${GOPATH}/src/${REPOPATH}" \
    TOOLDIR="${GOPATH}/src/${TOOLPATH}"

ARG OUTDIR
ENV OUTDIR="${OUTDIR:-"/.reports"}"

RUN mkdir -p "${REPODIR}" "${OUTDIR}"
COPY . "${REPODIR}"
WORKDIR "${REPODIR}"

### Put symlink refers submodule-path at origin-path
RUN ln -s "${REPODIR}/.gitmodules.d/$(basename "${TOOLPATH}")" "${TOOLDIR}"

### Run yamllint ...
RUN yamllint -f parsable . > "${OUTDIR}/yamllint.issues" || true
RUN ls -la "${OUTDIR}"

### Convert yamllint issues to SARIF ...
RUN go run "${TOOLDIR}/yamllint/cmd/main.go" "${REPOPATH}" \
        < "${OUTDIR}/yamllint.issues" \
        > "${OUTDIR}/yamllint.sarif"
RUN ls -la "${OUTDIR}"
