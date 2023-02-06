
```sql
-- author：lgh
-- time：2021年3月12日
-- vision：1.3

/*
------------------------------------------------
点表：               边表：
1. 创建学者点表       1.  创建学者-组织边表 
2. 创建组织点表       2.  创建学者-学科边表
3. 创建学科点表       3.  创建学者-专利边表
4. 创建专利点表       4.  创建学者-项目边表
5. 创建项目点表       5.  创建学者-论文边表
6. 创建论文点表       6.  创建专利-专利边表
------------------------------------------------
1. 点边表字段名称类型调整
2. 表的类型转换ORC-TORC
*/

--学者点表
--连接“论文id”与“是否是第一作者字段” 
CREATE TABLE kg_data.tem_user_all_paper AS SELECT 
userid,
concat_ws(':',paperid,whetherfirstwriter) AS paper_whetherfirstwriter
FROM std_data.t03_paperuserrela; 
--合并同一作者的论文信息
CREATE TABLE kg_data.user_all_paper AS SELECT 
userid,
concat_ws('|',collect_set(paper_whetherfirstwriter)) AS all_paper
FROM kg_data.tem_user_all_paper
GROUP BY userid;
--关联学者基本信息表、学者联系方式、学者所属组织、学者所涉足学科、学者h指数、学者发表的论文信息
create table kg_data.tem1_node_user as select 
a.userid, 
a.userengname, 
a.userchnname, 
a.birthday,
b.contact,
a.papernum,
a.citnum,
a.userrorcid, 
a.orgid,
c.orgengname,
c.orgchnname,
d.indicatorvalue,
e.subjectcode, 
f.all_paper
from std_data.t01_user a
left join std_data.t01_usercontact b on a.userid = b.userid
left join std_data.t02_org c on a.orgid = c.orgid
left join (SELECT userid,indicatorvalue FROM std_data.ind_indicatorstatis WHERE indicatorid ='IND0000000001') d 
on a.userid = d.userid 
left join std_data.t01_usersubject e on a.userid = e.userid
LEFT JOIN kg_data.user_all_paper f ON a.userid = f.userid;
--通过学者-学科联系表关联学科表获取相关学科信息
create table kg_data.tem2_node_user as select 
a.*,
b.subjectengname,
b.subjectchnname
from tem1_node_user a
left join std_data.t00_subcode b on a.subjectcode = b.subjectcode;
--合并同一学者的学科、组织信息
CREATE TABLE node_user AS SELECT  
userid,userengname,userchnname,birthday,contact,papernum,citnum,userrorcid,indicatorvalue,all_paper,
concat_ws(',',collect_set(subjectcode)) as all_subjectcode,
concat_ws('|',collect_set(subjectengname)) AS all_subjectengname,
concat_ws('|',collect_set(subjectchnname)) AS all_subjectchnname,
concat_ws('|',collect_set(orgid)) AS all_orgid,
concat_ws('|',collect_set(orgengname)) AS all_orgengname,
concat_ws('|',collect_set(orgchnname)) AS all_orgchnname
from kg_data.tem2_node_user
group by 
userid,userengname,userchnname,birthday,contact,papernum,citnum,userrorcid,indicatorvalue,all_paper;

--组织点表
--关联组织信息、组织别名、组织联系方式、所在国家省市编号
create table tem1_node_org AS select  
a.orgid,
a.orgengname,
a.orgchnname,
b.organthername,
a.orgtypecode,
c.orgtypedetail,
cast(d.contact as string) as contact,
a.citycode,
e.provincecode,
e.countrycode,
e.cityengname,
e.citychnname
from std_data.t02_org a
left join std_data.t02_organthername b on a.orgid = b.orgid
left join std_data.t00_orgtypecode c on a.orgtypecode = c.orgtypecode
left join std_data.t02_orgcontact d on a.orgid = d.orgid
left join std_data.t00_city e on a.citycode = e.citycode;
--关联国家省份名称
create table tem2_node_org AS select
a.*,
b.provinceengname,
b.provincechnname,
c.countryengname,
c.countrychnname
from tem1_node_org a
left join std_data.t00_province b on a.provincecode = b.provincecode
left join std_data.t00_country c on a.countrycode = c.countrycode;
--合并同个组织的别名信息
CREATE TABLE node_org AS SELECT  
orgid,orgengname,orgchnname,orgtypedetail,contact,citycode,provincecode,countrycode,
cityengname,citychnname,provinceengname,provincechnname,countryengname,countrychnname,
concat_ws('|',collect_set(organthername)) as all_organthername
from kg_data.tem2_node_org
group by orgid,orgengname,orgchnname,orgtypedetail,contact,citycode,provincecode,countrycode,
cityengname,citychnname,provinceengname,provincechnname,countryengname,countrychnname;

--学科点表
create table kg_data.node_subject AS select 
subjectcode,
subjectengname,
subjectchnname,
subjectengshort,
subjectchnshort
from std_data.t00_subcode;

--专利点表
--关联专利信息与其法律标志信息
CREATE TABLE node_patent AS SELECT 
a.annid,
a.patappid,
a.patappdate,
a.patanndate,
a.patabs,
a.foreabs,
a.transabs,
a.pattitle,
a.foretitle,
a.transtitle,
a.patapplicant,
a.appfullname,
a.appaddress,
a.appcountry,
a.appccity,
a.apptypecode,
a.inventor,
a.intproagy,
a.intproagt,
a.locarno,
a.firstanndate,
a.stansign,
a.chnpatsign,
a.customsign,
a.interpattypeid,
a.pctannid,
a.pctpatappid,
b.coopersign,
b.appsign,
b.rejsign,
b.authsign,
b.persign,
b.transsign,
b.lawsuitsign,
b.pledgesign,
b.invalidsign
FROM std_data.t04_patent a
LEFT JOIN  std_data.t04_patlaw b ON a.annid=b.annid;  AUTO_INCREMENT 

--论文点集
--关联论文信息、期刊信息、会议信息、论文类型描述与论文第一作者信息
CREATE TABLE kg_data.node_paper AS SELECT 
a.paperid,
a.papertitle,
a.papersummary,
a.pubyear,
a.srcjouid,
b.jouengname,
b.jouchnname,
a.srcconferid,
c.confername,
c.conferperiod,
a.citnum,
a.papertypecode,
d.papertypedetail,
a.firstwriteruserid,
e.userengname,
e.userchnname
FROM std_data.t03_paper a
LEFT JOIN std_data.t03_journal b ON a.srcjouid = b.jouid
LEFT JOIN std_data.t03_confer c ON a.srcconferid = c.conferid
LEFT JOIN std_data.t00_papertypecode d on a.papertypecode = d.papertypecode
LEFT JOIN std_data.t01_user e ON a.firstwriteruserid = e.userid;   

--项目点集
--关联项目信息与资助组织信息
CREATE TABLE node_fundproject AS SELECT 
a.projectid,
a.projectname,
a.projectsimname,
a.projectdetail,
a.projectfundno,
a.suporgcode,
b.suporgdetail
FROM std_data.t05_fundproject a
LEFT JOIN std_data.t00_suporgcode b ON a.suporgcode = b.suporgcode;  AUTO_INCREMENT 

--学者-学科边集
CREATE TABLE node_subject AS SELECT
subjectcode,
subjectengname,
subjectchnname,
subjectengshort,
subjectchnshort
FROM std_data.t00_subcode;

--学者-组织边集
--连接学者组织任职的开始与结束时间组成任职期间信息
create table tem1_edge_user_org as select 
a.userid,
a.orgid,
concat_ws(',',cast(startdate AS string),cast(enddate AS string)) AS work_date
from std_data.t01_userorghis a;
# 合并学者在同个组织的任职期间
CREATE TABLE  kg_data.edge_user_org AS SELECT 
userid,
orgid,
concat_ws('|',collect_set(work_date)) as all_date
FROM tem1_edge_user_org
GROUP BY userid,orgid;

--学者-专利边表
CREATE TABLE kg_data.edge_user_patent AS SELECT 
annid,
userid
FROM std_data.t04_patentuserrela;  

--学者-项目边表
--通过论文找出学者与项目的关联
CREATE TABLE userfundprojectrela AS SELECT 
a.paperid,
a.projectid,
b.userid
FROM std_data.t03_paperfundprojectrela a
LEFT JOIN std_data.t03_paperuserrela b ON a.paperid = b.paperid
--关联学者与项目，并合并同个学者同个项目中包含的论文信息
CREATE TABLE kg_data.edge_user_fundproject AS SELECT
userid,
projectid,
concat_ws('|',collect_list(paperid)) AS all_paper
FROM kg_data.userfundprojectrela
GROUP BY userid,projectid;  

--学者-论文边集
CREATE TABLE kg_data.edge_user_paper AS SELECT 
paperid,
userid,
whetherfirstwriter
FROM std_data.t03_paperuserrela;   

--专利-同族关系(同篇专利在不同国家申请专利)
--寻找专利间的同族关系 
create table edge_patent_family_patent as select a.simfam, a.annid as annid1, b.annid as annid2
from std_data.t04_patentfamrela as a,
std_data.t04_patentfamrela as b
where a.simfam = b.simfam and a.annid > b.annid;  

--点边表字段名称类型调整
--学者点集字段名
ALTER TABLE kg_data.node_user ADD COLUMNS (TABLE_type STRING COMMENT '表格类型');
ALTER TABLE kg_data.node_user CHANGE userid 学者编号 varchar(50);
ALTER TABLE kg_data.node_user CHANGE userengname 学者英文名称 varchar(255);
ALTER TABLE kg_data.node_user CHANGE userchnname 学者中文名称 varchar(255);
ALTER TABLE kg_data.node_user CHANGE birthday 出生日期 DATE;
ALTER TABLE kg_data.node_user CHANGE contact 联系方式 STRING;
ALTER TABLE kg_data.node_user CHANGE  出文量 发文量 int;
ALTER TABLE kg_data.node_user CHANGE citnum 被引次数 int;
ALTER TABLE kg_data.node_user CHANGE userrorcid 学者ORCID varchar(50);
ALTER TABLE kg_data.node_user CHANGE indicatorvalue h指数 int;
ALTER TABLE kg_data.node_user CHANGE all_paper 发表论文及位次 STRING;
ALTER TABLE kg_data.node_user CHANGE all_subjectcode 所属学科领域编号 STRING;
ALTER TABLE kg_data.node_user CHANGE all_subjectengname 所属学科英文名称 STRING;
ALTER TABLE kg_data.node_user CHANGE all_subjectchnname 所属学科中文名称 STRING;
ALTER TABLE kg_data.node_user CHANGE all_orgid 所属组织编号 STRING;
ALTER TABLE kg_data.node_user CHANGE all_orgengname 所属组织英文名称 STRING;
ALTER TABLE kg_data.node_user CHANGE all_orgchnname 所属组织中文名称 STRING;

--学科点集字段名
ALTER TABLE kg_data.node_subject CHANGE subjectcode 学科领域编号 varchar(50);
ALTER TABLE kg_data.node_subject CHANGE subjectengname 学科英文名称  varchar(255);
ALTER TABLE kg_data.node_subject CHANGE subjectchnname 学科中文名称  varchar(255);
ALTER TABLE kg_data.node_subject CHANGE subjectengshort 学科英文简称  VARCHAR(255);
ALTER TABLE kg_data.node_subject CHANGE subjectchnshort 学科中文简称  varchar(255);

--组织点集字段名
ALTER TABLE kg_data.node_org CHANGE orgid 组织编号 varchar(50);
ALTER TABLE kg_data.node_org CHANGE orgengname 组织英文名称 varchar(255);
ALTER TABLE kg_data.node_org CHANGE orgchnname 组织中文名称 varchar(255);
ALTER TABLE kg_data.node_org CHANGE orgtypedetail 组织类型描述 varchar(255);
ALTER TABLE kg_data.node_org CHANGE contact 组织联系方式 STRING;
ALTER TABLE kg_data.node_org CHANGE citycode 城市编号 varchar(50);
ALTER TABLE kg_data.node_org CHANGE provincecode 省份编号 varchar(50);
ALTER TABLE kg_data.node_org CHANGE countrycode 国家编号 varchar(50);
ALTER TABLE kg_data.node_org CHANGE cityengname 城市英文名称 varchar(255);
ALTER TABLE kg_data.node_org CHANGE citychnname 城市中文名称 varchar(255);
ALTER TABLE kg_data.node_org CHANGE provinceengname 省份英文名称 VARCHAR(255);
ALTER TABLE kg_data.node_org CHANGE provincechnname 省份中文名称 varchar(255);
ALTER TABLE kg_data.node_org CHANGE countryengname 国家英文名称 varchar(255);
ALTER TABLE kg_data.node_org CHANGE countrychnname 国家中文名称 varchar(255);
ALTER TABLE kg_data.node_org CHANGE all_organthername 组织别称 STRING;

--论文点集字段名称
ALTER TABLE kg_data.node_paper CHANGE paperid 论文编号 varchar(50);
ALTER TABLE kg_data.node_paper CHANGE papertitle 论文标题 varchar(255);
ALTER TABLE kg_data.node_paper CHANGE papersummary 论文摘要 STRING;
ALTER TABLE kg_data.node_paper CHANGE pubyear 发表年份 varchar(4);
ALTER TABLE kg_data.node_paper CHANGE srcjouid 来源期刊编号 varchar(50);
ALTER TABLE kg_data.node_paper CHANGE jouengname 期刊英文名称 varchar(255);
ALTER TABLE kg_data.node_paper CHANGE jouchnname 期刊中文名称 varchar(255);
ALTER TABLE kg_data.node_paper CHANGE srcconferid 来源会议编号 varchar(50);
ALTER TABLE kg_data.node_paper CHANGE confername 来源会议名称 varchar(255);
ALTER TABLE kg_data.node_paper CHANGE conferperiod 会议期间 STRING;
ALTER TABLE kg_data.node_paper CHANGE citnum 被引次数 INT;
ALTER TABLE kg_data.node_paper CHANGE papertypecode 论文类型编号 varchar(50);
ALTER TABLE kg_data.node_paper CHANGE papertypedetail 论文类型描述 STRING;
ALTER TABLE kg_data.node_paper CHANGE firstwriteruserid 第一作者编号 varchar(50);
ALTER TABLE kg_data.node_paper CHANGE userengname 第一作者英文名称 varchar(255);
ALTER TABLE kg_data.node_paper CHANGE userchnname 第一作者中文名称 varchar(255);   

--项目点集字段
ALTER TABLE kg_data.node_fundproject CHANGE projectid 基金项目编号 varchar(50);
ALTER TABLE kg_data.node_fundproject CHANGE projectname 基金项目名称 varchar(255);
ALTER TABLE kg_data.node_fundproject CHANGE projectsimname 基金项目简称 varchar(255);
ALTER TABLE kg_data.node_fundproject CHANGE projectdetail 基金项目描述  STRING;
ALTER TABLE kg_data.node_fundproject CHANGE projectfundno 基金项目顺序 varchar(50);
ALTER TABLE kg_data.node_fundproject CHANGE suporgcode 资助组织编号 varchar(50);
ALTER TABLE kg_data.node_fundproject CHANGE suporgdetail 资助组织描述 VARCHAR(255);

--专利点集字段
ALTER TABLE kg_data.node_patent CHANGE annid 公告号 varchar(50);
ALTER TABLE kg_data.node_patent CHANGE patappid 专利申请号 varchar(50);
ALTER TABLE kg_data.node_patent CHANGE patappdate 申请日期 DATE;
ALTER TABLE kg_data.node_patent CHANGE patanndate 公告日期 DATE;
ALTER TABLE kg_data.node_patent CHANGE pattitle 专利标题 STRING;
ALTER TABLE kg_data.node_patent CHANGE foretitle 外文标题 STRING;
ALTER TABLE kg_data.node_patent CHANGE transtitle 经翻译的标题 STRING;
ALTER TABLE kg_data.node_patent CHANGE patapplicant 申请人 STRING;
ALTER TABLE kg_data.node_patent CHANGE appfullname 申请人全名 STRING;
ALTER TABLE kg_data.node_patent CHANGE appaddress 申请人地址 STRING;
ALTER TABLE kg_data.node_patent CHANGE appcountry 申请人国家 STRING;
ALTER TABLE kg_data.node_patent CHANGE appccity 申请人省市 STRING;
ALTER TABLE kg_data.node_patent CHANGE apptypecode 申请人类型编号 varchar(50);
ALTER TABLE kg_data.node_patent CHANGE inventor 发明人 varchar(255);
ALTER TABLE kg_data.node_patent CHANGE intproagy 知识产权代理机构  varchar(255);
ALTER TABLE kg_data.node_patent CHANGE intproagt 知识产权代理人 VARCHAR(255);
ALTER TABLE kg_data.node_patent CHANGE locarno 洛迦诺分类号 varchar(50);
ALTER TABLE kg_data.node_patent CHANGE firstanndate 首次公开日  DATE;
ALTER TABLE kg_data.node_patent CHANGE stansign 标准专利标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE chnpatsign 中国专利标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE customsign 海关专利标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE interpattypeid 国际专利类型号 STRING;
ALTER TABLE kg_data.node_patent CHANGE pctannid PCT国际申请申请号  varchar(255);
ALTER TABLE kg_data.node_patent CHANGE pctpatappid PCT国际公告申请号  varchar(255);
ALTER TABLE kg_data.node_patent CHANGE coopersign 合作标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE appsign 申请标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE rejsign 拒绝标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE authsign 授权标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE persign 许可标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE transsign 转让标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE lawsuitsign 诉讼标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE pledgesign 质押标志 varchar(1);
ALTER TABLE kg_data.node_patent CHANGE invalidsign 无效标志 varchar(1);

--学者-组织字段
ALTER TABLE kg_data.edge_user_org CHANGE userid 学者编号 varchar(50);
ALTER TABLE kg_data.edge_user_org CHANGE orgid 组织编号 varchar(50);
ALTER TABLE kg_data.edge_user_org CHANGE all_date 任职日期 string;

--学者-学科字段
ALTER TABLE kg_data.edge_user_subject CHANGE userid 学者编号 varchar(50);
ALTER TABLE kg_data.edge_user_subject CHANGE subjectcode 学科编号 varchar(50); 

--学者-论文字段
ALTER TABLE kg_data.edge_user_paper CHANGE userid 学者编号 varchar(50);
ALTER TABLE kg_data.edge_user_paper CHANGE paperid 论文编号 varchar(50);
ALTER TABLE kg_data.edge_user_paper CHANGE whetherfirstwriter 是否是第一作者 varchar(1);

--学者-项目字段
ALTER TABLE kg_data.edge_user_fundproject CHANGE userid 学者编号 varchar(50);
ALTER TABLE kg_data.edge_user_fundproject CHANGE projectid 基金项目编号 varchar(50);
ALTER TABLE kg_data.edge_user_fundproject CHANGE all_paper 项目论文编号 string;

--学者-专利字段
ALTER TABLE kg_data.edge_user_patent CHANGE userid 学者编号 varchar(50);
ALTER TABLE kg_data.edge_user_patent CHANGE annid 公告号 varchar(50);

--专利-同族字段
ALTER TABLE kg_orc.edge_patent_family_patent CHANGE annid1 公告号1 varchar(50);
ALTER TABLE kg_orc.edge_patent_family_patent CHANGE annid2 公告号2 varchar(50);
ALTER TABLE kg_orc.edge_patent_family_patent CHANGE simfam 同族号  string;

--表的类型转换ORC-TORC
CREATE TABLE kg_orc.node_subject STORED AS ORC AS SELECT * FROM kg_data.node_subject;
CREATE TABLE kg_orc.node_user STORED AS ORC AS SELECT * FROM kg_data.node_user;
CREATE TABLE kg_orc.node_org STORED AS ORC AS SELECT * FROM kg_data.node_org;
CREATE TABLE kg_orc.node_paper STORED AS ORC AS SELECT * FROM kg_data.node_paper; 
CREATE TABLE kg_orc.node_patent STORED AS ORC AS SELECT * FROM kg_data.node_patent; 
CREATE TABLE kg_orc.node_fundproject STORED AS ORC AS SELECT * FROM kg_data.node_fundproject; 
CREATE TABLE kg_orc.edge_user_paper STORED AS ORC AS SELECT * FROM kg_data.edge_user_paper;
CREATE TABLE kg_orc.edge_user_org STORED AS ORC AS SELECT * FROM kg_data.edge_user_org; 
CREATE TABLE kg_orc.edge_user_subject STORED AS ORC AS SELECT * FROM kg_data.edge_user_subject; 
CREATE TABLE kg_orc.edge_user_fundproject STORED AS ORC AS SELECT * FROM kg_data.edge_user_fundproject; 
CREATE TABLE kg_orc.edge_user_patent STORED AS ORC AS SELECT * FROM kg_data.edge_user_patent;   
CREATE TABLE kg_orc.edge_patent_family_patent STORED AS ORC AS SELECT * FROM kg_data.edge_patent_family_patent;    
```