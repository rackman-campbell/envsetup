#Layer 1
FROM centos:centos8

#Layer 2 downloads and prep config
RUN curl -l https://releases.hashicorp.com/terraform/0.12.10/terraform_0.12.10_linux_amd64.zip > ./terraform_0.12.10_linux_amd64.zip \
&& curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl \
&& curl -o aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.14.6/2019-08-22/bin/linux/amd64/aws-iam-authenticator \
&& curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp \
&& curl -l https://s3.amazonaws.com/faws-toolbox.rax.io/v2.1.0/faws-linux.zip > ~/faws-darwin.zip \
&& mkdir /hostsync

#Layer 3 packages
RUN yum -y install python36 \
wget \
unzip \
git \
jq \
&& pip3 install awscli --upgrade

#Layer 4 executables
RUN unzip ./terraform_0.12.10_linux_amd64.zip \
&& mv terraform /usr/local/bin/ \
&& chmod +x ./kubectl \
&& mv ./kubectl /usr/local/bin/kubectl \
&& chmod +x ./aws-iam-authenticator \
&& mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$HOME/bin:$PATH \
&& echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc \
&& mv /tmp/eksctl /usr/local/bin \
&& unzip -j ~/faws-darwin.zip faws -d /usr/local/bin/

#Layer 5
CMD /bin/bash
