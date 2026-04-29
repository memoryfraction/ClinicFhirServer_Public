
# ClinicFHIRServer User Manual

## Table of Contents
- [English Documentation](#english-documentation)
- [Chinese Documentation](#chinese-documentation)

## English Documentation

0 Abstract
This document, based on the source code and adhering to the principles of objectivity, rigor, and compliance, provides a user manual for ClinicFhirServer. It aims to help readers understand the project's development purpose, the problems it addresses for whom, and how to use it.
ClinicFHIRServer partially utilizes code from FirelyTeam/Spark (BSD-3-Clause License). This part implements Swagger and the basic FHIR standard. However, a production-level FHIR project requires high-level HIPAA requirements, such as RBAC access control, 99.9% SLA, encryption of sensitive data like PHI, disaster recovery, and data not leaving the country. ClinicFHIRServer relies on cloud architectures and cloud databases like Azure and Neon, implementing mandatory features such as high availability, strong encryption, and RBAC access control.

1. Project Background
Healthcare institutions face a common challenge in the process of digitalization: how to achieve data interoperability with the FHIR standard while meeting HIPAA compliance requirements?
While open-source FHIR servers (such as FirelyTeam/Spark) are available on the market and can quickly build APIs compliant with the FHIR R4 standard, they still lack key capabilities for production-grade healthcare systems.


ClinicFHIRServer targets small and medium-sized clinics and healthcare SaaS developers who need to quickly access FHIR compliance capabilities but lack the resources to build a complete security system themselves.



2 Online Demo
https://clinic-fhir-server-app.blackdesert-8e20099d.eastasia.azurecontainerapps.io/

2.1 Demo Account
After accessing the login page, click the corresponding role button to fill in the information with one click. The password is always "Demo2026"!


Enterprise employees can sign in using their Azure AD accounts by signing in with Microsoft.

2.2 Demo Account Role-Playing
2.2.1 TenantAdmin — Clinic Administrator
Managing this tenant's users, without PHI access permissions.




2.2.2 Physician
Access to patient, consultation, and observation data

Physician Login

Physician Dashboard


Patients List

Patient Details
2.2.3 Patient — Nurse
Access to your own patient and observation data

3 Core Functions
3.1 RBAC Access Control
The system has multiple built-in roles and strictly adheres to HIPAA.Minimum Necessity Principle：
SysAdmin — Platform administrator, no PHI access privileges.
TenantAdmin — Clinic administrator, manages users within this tenant, no PHI access.
Physician — Doctors can access patient, consultation, and observation data.
Nurse — a nurse who can access patient and observation data.
FrontDesk — The front-end, which only allows access to basic patient information.
Biller
Auditor — an auditor who provides read-only audit logs with automatic PHI anonymization.
Patient — The patient can only view their own records.

3.2 FHIR R4 API
Implement the standard FHIR R4 interface based on Spark, supporting resource types such as Patient, Encounter, and Observation.

3.3 Multi-tenant isolation
Each clinic (Tenant) has its own independent database, with completely isolated data, and is dual-verified through JWT tenantid claim and Finbuckle MultiTenant.

3.4 Safety and Compliance
The connection string is stored encrypted using Azure Key Vault + Managed Identity, and no plaintext keys are present in the container.
Audit logs are append-only and cannot be modified, meeting HIPAA's 6-year retention requirement.
Rate limits: Authentication interface 10 times/minute, FHIR interface 30 times/minute
Data is deployed on Azure, and the production environment is deployed on a US node to meet the requirement that data does not leave the country.

4. Technical Architecture

5. How to use
5.1. Experience the Demo
Access the link above directly, log in with any Demo account, and explore the corresponding role's interface and FHIR API.

5.2 Calling the FHIR API
After logging in, you can access the REST API via Swagger UI or by directly calling it.


5.3 Feedback
If you have any questions or suggestions during use, please feel free to provide feedback through the following methods:
https://docs.google.com/forms/d/e/1FAIpQLSdQyra9-QJhCScIuUVjfBaldFmlmK-tESpcGYdDiVl7GDVSBg/viewform

6. Disclaimer
This project is currently a demo version. All data is synthetic and does not involve any real PHI (Protected Health Information).

References
https://www.hhs.gov/hipaa/index.html
https://www.fhir.org/
https://www.fda.gov/regulatory-information/selected-amendments-fdc-act/21st-century-cures-act


## [Chinese Documentation](#chinese-documentation)

0 摘要
本文依据源代码，基于客观、严谨、合规的原则，对ClinicFhirServer写使用说明文档。使得读者阅读后可以理解该项目的开发初衷、解决哪些人的什么问题， 如何使用？等问题。
ClinicFHIRServer部分使用了FirelyTeam/Spark(BSD-3-Clause License)的代码, 该部分实现了Swagger和基础的FHIR标准， 但是一个生产级FHIR项目需要RBAC权限控制、99.9%SLA、PHI等敏感数据加密、灾难备份、数据不得离开国境等高级别的HIPAA要求。ClinicFHIRServer依赖与Azure和Neon等云架构和云数据库，实现了高可用、强加密、RBAC权限管控等强制特性。

1 项目背景
医疗机构在数字化过程中面临一个共同难题：如何在满足 HIPAA 合规要求的前提下，实现 FHIR 标准的数据互操作？
市面上已有开源 FHIR 服务器（如 FirelyTeam/Spark），能快速搭建符合 FHIR R4 标准的 API，但距离生产级医疗系统还缺少关键能力：


ClinicFHIRServer 的目标用户是需要快速接入 FHIR 合规能力、但没有资源自建完整安全体系的中小型诊所和医疗 SaaS 开发者。



2 在线 Demo
https://clinic-fhir-server-app.blackdesert-8e20099d.eastasia.azurecontainerapps.io/

2.1 Demo 账号
访问登录页后，点击对应角色按钮即可一键填入，密码统一为 Demo2026!

企业员工可通过 Sign in with Microsoft 使用 Azure AD 账号登录。

2.2 Demo账号角色扮演
2.2.1 TenantAdmin — 诊所管理员
管理本租户用户，无 PHI 访问权限




2.2.2 Physician — 医生
可访问患者、就诊、观察数据

Physician Login

Physician Dashboard


Patients List

Patient Details
2.2.3 Patient — 护士
可访问自己的患者和观察数据

3 核心功能
3.1 RBAC 权限控制
系统内置多种角色，严格遵循 HIPAA 最小必要原则：
SysAdmin — 平台管理员，无 PHI 访问权限
TenantAdmin — 诊所管理员，管理本租户用户，无 PHI 访问权限
Physician — 医生，可访问患者、就诊、观察数据
Nurse — 护士，可访问患者和观察数据
FrontDesk — 前台，仅可访问患者基本信息
Biller — 账单员
Auditor — 审计员，只读审计日志，PHI 自动脱敏
Patient — 患者，仅可查看本人记录

3.2 FHIR R4 API
基于 Spark 实现标准 FHIR R4 接口，支持 Patient、Encounter、Observation 等资源类型。

3.3 多租户隔离
每个诊所（Tenant）拥有独立的数据库，数据完全隔离，通过 JWT tenantid claim 和 Finbuckle MultiTenant 双重验证。

3.4 安全与合规
连接字符串通过 Azure Key Vault + Managed Identity 加密存储，容器内无任何明文密钥
审计日志 Append-only，不可修改，满足 HIPAA 6 年留存要求
速率限制：认证接口 10次/分钟，FHIR 接口 30次/分钟
数据部署在 Azure，生产环境会部署再美国节点，满足数据不出境要求

4 技术架构

5 如何使用
5.1. 体验 Demo
直接访问上方链接，选择任意 Demo 账号登录，探索对应角色的功能界面和 FHIR API。

5.2 调用 FHIR API
登录后，通过 Swagger UI 或直接调用 REST API：


5.3 Feedback
使用过程中有任何问题或建议，欢迎通过以下方式反馈：
https://docs.google.com/forms/d/e/1FAIpQLSdQyra9-QJhCScIuUVjfBaldFmlmK-tESpcGYdDiVl7GDVSBg/viewform

6 免责声明 / Disclaimer 
本项目目前为 Demo 版本，所有数据均为合成假数据，不涉及任何真实 PHI（受保护健康信息）。 This is a demo deployment. All data is synthetic. No real PHI is involved.

参考资料
https://www.hhs.gov/hipaa/index.html
https://www.fhir.org/
https://www.fda.gov/regulatory-information/selected-amendments-fdc-act/21st-century-cures-act

