FROM jenkins/inbound-agent:latest
USER root
RUN apt-get update && \
    apt-get install -y ansible curl unzip python3-pywinrm python3-requests-ntlm && \
    rm -rf /var/lib/apt/lists/*
RUN curl -fsSL https://releases.hashicorp.com/terraform/1.9.5/terraform_1.9.5_linux_amd64.zip -o /tmp/tf.zip && \
    unzip /tmp/tf.zip -d /usr/local/bin && rm /tmp/tf.zip
RUN ansible-galaxy collection install ansible.windows community.windows chocolatey.chocolatey \
    -p /usr/share/ansible/collections
USER jenkins

docker build -t jenkins-agent-iac:latest .
docker run --rm --entrypoint sh jenkins-agent-iac:latest -c "ansible-galaxy collection list | grep -E 'windows|chocolatey'; python3 -c 'import winrm; print(\"pywinrm ok\")'"

