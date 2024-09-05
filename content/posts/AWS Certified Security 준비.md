---
title: "AWS Certified Security 준비_문제풀이"
date: 2024-06-14
# weight: 1
# aliases: ["/AWS Certified Security"]
tags: ["AWS Certified Security", "Cloud Security"]
author: "CrackerNote"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
disableHLJS: false # to disable highlightjs
disableShare: true
hideSummary: false
searchHidden: true
ShowReadingTime: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: false
ShowRssButtonInSectionTermList: true
UseHugoToc: false
searchHidden: true
---

## 1️⃣ AWS Certified Security 준비_문제풀이

1. ExamTopics 문제 풀기
   (https://www.examtopics.com/exams/amazon/aws-certified-security-specialty/)

  

##### 📜**1번**

| 질문 | The Security team believes that a former employee may have gained unauthorized access to AWS resources sometime in the past 3 months by using an identified access key.<br/>What approach would enable the Security team to find out what the former employee may have done within AWS?<br />보안 팀은 이전 직원이 식별된 액세스 키를 사용하여 지난 3개월 동안 AWS 리소스에 대한 무단 액세스 권한을 얻었을 수 있다고 생각합니다. 이전 직원이 AWS 내에서 수행했을 수 있는 작업을 보안 팀이 찾을 수 있는 접근 방식은 무엇입니까? |
| :--- | ------------------------------------------------------------ |
| 보기 | A. Use the AWS CloudTrail console to search for user activity. <br />B. Use the Amazon CloudWatch Logs console to filter CloudTrail data by user. <br />C. Use AWS Config to see what actions were taken by the user. <br />D. Use Amazon Athena to query CloudTrail logs stored in Amazon S3. |
| 정답 | A                                                            |
| 풀이 | AWS CloudTrail은 AWS에서 제공하는 로깅 서비스로, AWS 계정에서 수행되는 API 호출 및 관리 작업을 모두 기록합니다. 이전 직원이 AWS 리소스에 대한 무단 액세스 권한을 얻었다면, 해당 직원이 AWS 리소스에 대한 API 호출 및 관리 작업을 수행했을 가능성이 높습니다. AWS CloudTrail을 사용하여 해당 직원의 API 호출 및 관리 작업을 검색하고 이를 분석하여 무단 액세스 권한을 얻었을 가능성이 높은 작업을 찾을 수 있습니다.   그러나 CloudWatch Logs, AWS Config, Amazon Athena도 유용한 도구이며, AWS 리소스에 대한 보안 검토 및 탐지를 위해 사용될 수 있습니다. CloudWatch Logs는 CloudTrail 로그를 보고 분석할 수 있으며, AWS Config는 AWS 리소스 구성을 검사하고 이를 모니터링하는 데 사용될 수 있습니다. Amazon Athena를 사용하여 S3에 저장된 CloudTrail 로그를 쿼리하면 다양한 데이터 분석 작업을 수행할 수 있습니다.<br /><br />[용어]<br />CloudTrail<br />CloutWatch |



##### 📜**2번**

| 질문 | A company is storing data in Amazon S3 Glacier. The security engineer implemented a new vault lock policy for 10TB of data and called initiate-vault-lock operation 12 hours ago. The audit team identified a typo in the policy that is allowing unintended access to the vault. What is the MOST cost-effective way to correct this?<br />회사에서 Amazon S3 Glacier에 데이터를 저장하고 있습니다. 보안 엔지니어는 10TB 데이터에 대한 새로운 볼트 잠금 정책을 구현하고 12시간 전에 시작-볼트-잠금 작업을 호출했습니다. 감사 팀은 볼트에 대한 의도하지 않은 액세스를 허용하는 정책의 오타를 식별했습니다. 이 문제를 해결하는 가장 비용 효율적인 방법은 무엇입니까? |
| :--- | ------------------------------------------------------------ |
| 보기 | A. Call the abort-vault-lock operation. Update the policy. Call the initiate-vault-lock operation again. <br />B. Copy the vault data to a new S3 bucket. Delete the vault. Create a new vault with the data. <br />C. Update the policy to keep the vault lock in place. <br />D. Update the policy. Call initiate-vault-lock operation again to apply the new policy. |
| 정답 | A                                                            |
| 풀이 | 저장소(볼트) 잠금 정책 시작 후, 잠금 정책 완료 작업을 수행하지 않았다면 24시간 동안 해당 정책에 대한 유효성 검증 및 삭제가 가능합니다. 따라서 감사 팀이 보안 엔지니어가 구현한 S3 Glacier 볼트 잠금 정책에서 오타를 식별했다면, 이 문제를 해결하기 위해 가장 비용 효율적인 방법은 다음과 같습니다.   <br />1. abort-vault-lock 작업을 호출하여 현재 볼트 잠금 설정을 취소합니다. <br />2. 정책을 업데이트하여 볼트에 대한 올바른 권한을 부여합니다. <br />3. initial-vault-lock 작업을 다시 호출하여 변경된 보안 정책을 기반으로 볼트를 다시 잠급니다.   <br />위의 세 가지 단계를 따르면 S3 Glacier 볼트 잠금 설정이 업데이트되고 새로운 권한이 적용될 수 있습니다. 이 방법은 볼트를 다시 생성하거나 데이터를 새 버킷으로 이동할 필요가 없으므로 비용 효율적인 방법입니다.<br /><br />[용어]<br />Amazon S3 Glacier<br />Vault |



##### 📜**3번**

| 질문 | A company wants to control access to its AWS resources by using identities and groups that are defined in its existing Microsoft Active Directory.<br/>What must the company create in its AWS account to map permissions for AWS services to Active Directory user attributes?<br />회사는 기존 Microsoft Active Directory에 정의된 자격 증명 및 그룹을 사용하여 AWS 리소스에 대한 액세스를 제어하려고 합니다. 회사는 AWS 서비스에 대한 권한을 Active Directory 사용자 속성에 매핑하기 위해 AWS 계정에서 무엇을 생성해야 합니까? |
| :--- | ------------------------------------------------------------ |
| 보기 | A. AWS IAM groups <br />B. AWS IAM users <br />C. AWS IAM roles <br />D. AWS IAM access keys |
| 정답 | C                                                            |
| 풀이 | AWS 리소스에 대한 액세스를 제어하려는 경우, AWS 계정에서 Active Directory 사용자와 AWS 서비스의 권한을 매핑하려면 AWS IAM 역할을 생성해야 합니다.   <br />IAM 역할은 AWS 리소스에 대한 액세스를 제어하는 데 사용되는 AWS Identity and Access Management(IAM)의 기능 중 하나입니다. IAM 역할은 다른 AWS 계정, AWS 서비스 또는 외부 ID(예: Microsoft Active Directory 사용자)에 의해 사용될 수 있습니다. IAM 역할은 AWS 리소스에 대한 권한을 정의하는 IAM 정책을 가지고 있습니다. 이러한 IAM 정책은 AWS 리소스에 대한 권한 부여 규칙을 지정하는 데 사용됩니다.   <br />따라서, 회사가 기존 Microsoft Active Directory에 정의된 자격 증명 및 그룹을 사용하여 AWS 리소스에 대한 액세스를 제어하려는 경우, AWS 계정에서 IAM 역할을 생성하여 Active Directory 사용자 속성과 AWS 서비스의 권한을 매핑할 수 있습니다. IAM 역할은 Active Directory 사용자와 AWS 리소스 간에 중간 매개 역할을 수행할 수 있습니다.<br /><br />[용어]<br />IAM<br /> |



##### 📜**4번**

| 질문 | A company has contracted with a third party to audit several AWS accounts. To enable the audit, cross-account IAM roles have been created in each account targeted for audit. The Auditor is having trouble accessing some of the accounts.<br/>Which of the following may be causing this problem? (Choose three.)<br />한 회사가 여러 AWS 계정을 감사하기 위해 타사와 계약했습니다. 감사를 활성화하기 위해 감사 대상 각 계정에 교차 계정 IAM 역할이 생성되었습니다. 감사자가 일부 계정에 액세스하는 데 문제가 있습니다. 다음 중 이 문제를 일으킬 수 있는 것은 무엇입니까? (3개를 선택하세요.) |
| :--- | ------------------------------------------------------------ |
| 보기 | A. The external ID used by the Auditor is missing or incorrect. <br />B. The Auditor is using the incorrect password. <br />C. The Auditor has not been granted sts:AssumeRole for the role in the destination account. <br />D. The Amazon EC2 role used by the Auditor must be set to the destination account role. <br />E. The secret key used by the Auditor is missing or incorrect. <br />F. The role ARN used by the Auditor is missing or incorrect. |
| 정답 | A, C, F                                                      |
| 풀이 | 감사자가 일부 계정에 액세스하는 데 문제가 있을 때 발생할 수 있는 원인은 다음과 같습니다.   <br />C. 대상 계정의 역할에 대해 감사자에게 sts:AssumeRole이 부여되지 않았습니다. <br />F. 감사자가 사용하는 역할 ARN이 누락되었거나 올바르지 않습니다. <br />A. Auditor가 사용하는 외부 ID가 없거나 잘못되었습니다.   <br />따라서 C, F, A 세 가지 옵션이 이 문제를 일으킬 수 있는 가능성이 높습니다.   <br />B는 잘못된 비밀번호를 사용할 경우에는 해당 IAM 사용자 또는 역할에 대한 액세스 권한이 거부되지만, IAM 역할에 대한 교차 계정 액세스를 위해 비밀번호를 사용하는 것이 아니기 때문에 이 옵션은 이 문제를 일으키는 원인이 아닙니다.   D는 Amazon EC2 인스턴스에서 실행되는 IAM 역할은 대상 계정 역할로 설정되어야 하지만, 이 문제의 문제점은 AWS 계정 간의 IAM 역할을 통한 교차 계정 액세스이므로, D는 이 문제를 일으키는 원인이 아닙니다.   E는 잘못된 비밀 키를 사용하면 액세스 권한이 거부되므로 이 옵션은 문제의 원인이 될 수 있지만, 이 문제의 원인이 될 가능성은 상대적으로 낮습니다.<br /><br />[용어]<br />IAM<br /> |
