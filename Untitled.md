```
CREATE EXTERNAL TABLE "exp_wechat" (
"id" varchar(64),
"time1" varchar(128),
"uId" varchar(64),
"code" varchar(64),
"type" varchar(64),
"photoAlbumId" varchar(64),
"oId" varchar(64),
"wechatid" varchar(64),
"uri" varchar(500),
"accessType" varchar(64),
"ip" varchar(64),
"@version" varchar(64),
"@timestamp" varchar(64),
"host" varchar(64),
"port" varchar(100),
"from" varchar(100)
) LOCATION ('gpfdist://gpmaster137:8081/tmp.json') format 'text';
```

```

CREATE TABLE "wechat" (
"id" varchar(64),
"time1" varchar(64),
"uId" varchar(64),
"code" varchar(64),
"type" varchar(64),
"photoAlbumId" varchar(64),
"oId" varchar(64),
"wechatid" varchar(64),
"uri" varchar(500),
"accessType" varchar(64),
"ip" varchar(64),
"@version" varchar(64),
"@timestamp" varchar(64),
"host" varchar(64),
"port" varchar(100),
"from" varchar(100)
) DISTRIBUTED BY (id);
```

```
insert into wechat select data->'id', data->'time', data->'uid'->'code' from ext_wechat;
```

```
create external table ext_test (data json) location ('gpfdist://gpmaster137:8081/test.json') format 'text';

create table json_data  ( id text, type text, city text);
insert into test select data->'id', data->'type', data->'address'->'city' as city from ext_test;
```





```
CREATE EXTERNAL TABLE "exp_tb" (
"t1" varchar(64),
"t2" varchar(64),
"t3" varchar(64),
"t4" varchar(64),
"t5" varchar(64),
"t6" varchar(64),
"t7" varchar(64),
"t8" varchar(64),
"t9" varchar(64)
) LOCATION ('gpfdist://gpmaster137:8081/tmp.json') format 'text';
```

```
CREATE TABLE "tb" (
"t1" varchar(64),
"t2" varchar(64),
"t3" varchar(64),
"t4" varchar(64),
"t5" varchar(64),
"t6" varchar(64),
"t7" varchar(64),
"t8" varchar(64),
"t9" varchar(64));
```

```
insert into tb select data->'t1', data->'t2',  data->'t3', data->'t4', data->'t5', data->'t6', data->'t7', data->'t8', data->'t9', data->'t10', data->'t11', data->'t12', data->'t13',  data->'t14', data->'t15', data->'t16', data->'t17', data->'t18', data->'t19' from exp_tb;
```

```
#　报错
java.sql.BatchUpdateException: Batch entry 322 INSERT INTO es_wechat (time,code,photoalbumid,oid,wechatid,uri,orderid,accesstype,ip,fromstr,uid,platform,esid,estime )  VALUES (1516778177634,1,29643,29515,'AB0DDD7643734222A3B0D1BEA2722738','http://gallery.vphotos.cn/vphotosgallery/index.html?vphotowechatid=AB0DDD7643734222A3B0D1BEA2722738&from=singlemessage%20%E7%8E%B0%E5%9C%BA%E7%85%A7%E7%89%87%E4%BC%9A%E5%AE%9E%E6%97%B6%E6%9B%B4%E6%96%B0%EF%BC%8C%E5%A4%A7%E5%AE%B6%E5%8F%AF%E4%BB%A5%E9%9A%8F%E6%97%B6%E6%9F%A5%E7%9C%8B%EF%BC%8C%E4%B9%9F%E5%8F%AF%E4%BB%A5%E6%8B%89%E4%BD%8F%E6%91%84%E5%BD%B1%E5%B8%88%E5%A4%A7%E5%8F%94%E4%B8%80%E7%9B%B4%E6%8B%8D~%20%E5%B9%B4%E4%BC%9A%E6%98%AF%E4%BB%8A%E5%A4%A9%E4%B8%8B%E5%8D%882%E7%82%B9%E5%87%86%E6%97%B6%E5%BC%80%E5%A7%8B%E7%AD%BE%E5%88%B0%EF%BC%8C%E5%9B%A0%E4%B8%BA%E4%BC%9A%E5%9C%BA%E8%BF%98%E5%9C%A8%E5%B8%83%E7%BD%AE%EF%BC%8C%E8%AF%B7%E5%B7%B2%E7%BB%8F%E5%88%B0%E5%9C%BA%E7%9A%84%E4%BA%B2%E6%95%85%E4%BB%AC%E5%9C%A8%E5%A4%96%E7%AD%89%E5%80%99%E4%B8%80%E4%BC%9A%E5%84%BF%EF%BC%8C%E8%AE%B0%E5%BE%97%E7%A9%BF%E5%A5%BD%E5%A4%96%E8%A1%A3%EF%BC%8C%E4%B8%8D%E8%A6%81%E7%9D%80%E5%87%89~%20WiFi:meiguili%20%E5%AF%86%E7%A0%81:mgl1314520',NULL,1,'125.33.170.182, 10.3.16.51','singlemessage%20%E7%8E%B0%E5%9C%BA%E7%85%A7%E7%89%87%E4%BC%9A%E5%AE%9E%E6%97%B6%E6%9B%B4%E6%96%B0%EF%BC%8C%E5%A4%A7%E5%AE%B6%E5%8F%AF%E4%BB%A5%E9%9A%8F%E6%97%B6%E6%9F%A5%E7%9C%8B%EF%BC%8C%E4%B9%9F%E5%8F%AF%E4%BB%A5%E6%8B%89%E4%BD%8F%E6%91%84%E5%BD%B1%E5%B8%88%E5%A4%A7%E5%8F%94%E4%B8%80%E7%9B%B4%E6%8B%8D~%20%E5%B9%B4%E4%BC%9A%E6%98%AF%E4%BB%8A%E5%A4%A9%E4%B8%8B%E5%8D%882%E7%82%B9%E5%87%86%E6%97%B6%E5%BC%80%E5%A7%8B%E7%AD%BE%E5%88%B0%EF%BC%8C%E5%9B%A0%E4%B8%BA%E4%BC%9A%E5%9C%BA%E8%BF%98%E5%9C%A8%E5%B8%83%E7%BD%AE%EF%BC%8C%E8%AF%B7%E5%B7%B2%E7%BB%8F%E5%88%B0%E5%9C%BA%E7%9A%84%E4%BA%B2%E6%95%85%E4%BB%AC%E5%9C%A8%E5%A4%96%E7%AD%89%E5%80%99%E4%B8%80%E4%BC%9A%E5%84%BF%EF%BC%8C%E8%AE%B0%E5%BE%97%E7%A9%BF%E5%A5%BD%E5%A4%96%E8%A1%A3%EF%BC%8C%E4%B8%8D%E8%A6%81%E7%9D%80%E5%87%89~%20WiFi:meiguili%20%E5%AF%86%E7%A0%81:mgl1314520',15167781712023404,NULL,'AWEnBkTuEGFO5cEI10iH',1516778177634 ) was aborted: ERROR: value too long for type character varying(200)  (seg1 slice1 10.64.104.139:40000 pid=440383)  Call getNextException to see other errors in the batch.

```



```
change master to master_host='10.3.16.7',master_user='slave',master_password='vphotos',master_log_file='mysql-bin.000044',master_log_pos=31734296;
```





```
CREATE TABLE t_commission (id int8 primary key, user_id int8, commission_no text, commission_year int2, commission_month int2, amount int8, commission int8, rule_type int4, rule_arg0 text, diduct_type int4, diduct_arg0 text, diduct_arg1 text, state int4, status int4, comment text, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_credential (id int8 primary key, user_id int8, credential_name text, password text, union_id text, type int4, is_reset_password text, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_customer (id int8 primary key, user_id int8, old_user_id int8, role_id int8, state int4, authorization_number int4, role_number int4, salesman_id int8, customer_service_user_id int8, success_manager_user_id int8, user_code text, background_name text, background_phone text, background_url text, cus_price int8, file_name text, file_state int4, file_type int4, per_language int4, per_photographer int4, per_phosize int4, per_switch int4, personal_set text, user_category int4, merge_phototype int4, cus_price_model int4, recommend_id int8, recommend_type int4, cgi_id text, cgi_secret text, ding_corp_id text, ding_user_id text, filter_photographer int4, belong_to_user_id int8, ding_is_show_price int4, vip_level int4, crm_user_id text, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, company_type text, booking_set int4, static_set int4, ding_crop_id text, per_phographer int4, customer_type int4);

CREATE TABLE t_digital (id int8 primary key, user_id int8, state int4, source int4, type int4, user_code text, digital_bandwidth text, digital_config text, digital_key text, digital_price int8, mac_address text, terminal_permission text, photo_buffer_model_minutes int4, member_type int4, level int4, vphotos_level text, crm_user_id text, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_distribution (id int8 primary key, supplier_id int8, distributor_id int8, city_id int8, city text, qualify_date_start timestamp, qualify_date_end timestamp, protect_price int8, state int4, distribute_commission int8, purchase_commission int8, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_distribution_commission_rule (id int8 primary key, distribution_id int8, rule_type int4, rule_arg0 text, diduct_type int4, diduct_arg0 text, diduct_arg1 text, state int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_distributor (id int8 primary key, user_id int8, city_id int8, city text, state int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, code text);

CREATE TABLE t_login_record (id int8 primary key, role_id int8, credential_id int8, login_count int4, last_login_time timestamp, user_id int4, app_id int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_order_commission (id int8 primary key, order_id int8, commission_id int8, amount int8, deduct_amount int8, commission int8, state int4, status int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_organization_user (id int8 primary key, organization_user_id int8, user_id int8, organization_user_role_id int8, user_role_id int8, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, organization_role_id int4);

CREATE TABLE t_permission (id int8 primary key, name text, description text, resource_id int8, parent_id int8, type int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, lastId int4);

CREATE TABLE t_photographer (id int8 primary key, user_id int8, register_source int4, status int4, state int4, operation_specialist_user_id int8, type int4, user_code text, photographer_level text, photographer_price int8, source int4, v_box_deductible_amount int8, camera_type text, remark text, route4g_code text, sim_code text, wifi_code text, is_show int4, is_top int4, coverphoto text, is_init int4, physical_key_state int4, member_type int4, unique_id text, bis_audit int4, bis_audit_time timestamp, bis_audit_user int8, bis_user int8, company_audit int4, company_audit_time timestamp, company_audit_user int8, tech_audit int4, tech_audit_time timestamp, tech_audit_user int8, localhost_ip text, ssid text, vphotos_level text, crm_user_id text, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, local_ip text);

CREATE TABLE t_rank (id int8 primary key, name text, level int4, type int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, rankId int8, userId int8);

CREATE TABLE t_resource (id int8 primary key, name text, url text, element text, description text, resource_flag int4, show_flag int4, resource_tag text, group_id int8, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, lastId int4);

CREATE TABLE t_user_role (id int8 primary key, user_id int8, role_id int8, role_code text, state int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, roleCode text, roleName text);

CREATE TABLE t_role_permission (id int8 primary key, role_id int8, permission_id int8, role_type int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_salesman (id int8 primary key, user_id int8, state int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, code text);

CREATE TABLE t_service_provider (id int8 primary key, user_id int8, state int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, code text);

CREATE TABLE t_supplier (id int8 primary key, user_id int8, state int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, code text);

CREATE TABLE t_user (id int8 primary key, old_user_id int8, user_type int4, name text, nickname text, phone text, gender int4, city_id int8, city text, contact_person text, email text, birthday timestamp, address text, id_card_no text, company text, job_title text, validate_email int4, validate_phone int4, qq text, company_abbreviation text, company_telephone text, avatar text, profile text, qr_code text, state int4, customer_service_phone text, customer_service_telephone text, is_customer int4, crm_user_id text, title_id int8, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_user_rank (id int8 primary key, user_id int8, rank_id int8, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);

CREATE TABLE t_user_role (id int8 primary key, user_id int8, role_id int8, role_code text, state int4, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4, roleCode text, roleName text);

CREATE TABLE t_role (id int8 primary key, name text, description text, role_code text, cr_user_id int8, cr_date timestamp, op_user_id int8, op_date timestamp, del_tag int4, version int4);
```

```
CREATE TABLE v_tbl_digital_order (ID int4 primary key, AMOUNT_PAID float8, AMOUNT_PAYABLE float8, CREATE_TIME timestamp, DIGITAL_ID text, DIGITAL_ORDER_STATE int4, ORDER_ID int4, STATE int4, UPDATE_TIME timestamp, ATTITUDE_SCORE int4, SHOOTING_SCORE int4, TIMELINESS_SCORE int4, ORDER_TEMPLATE_ID int4, INVITATION_CODE text, SETTLE_WAY int4);

CREATE TABLE v_tbl_photographer_order (ID int4 primary key, PHOTOGRAPHER_ID text, ORDER_ID int4, AMOUNT_PAYABLE numeric, AMOUNT_PAID numeric, STATE int4, PHOTOGRAPHER_ORDER_STATE int4, ORDER_CREATE_TIME timestamp, ORDER_UPDATE_TIME timestamp, ORDER_UPLOAD_TIME timestamp, ATTITUDE_SCORE int4, SHOOTING_SCORE int4, TIMELINESS_SCORE int4, ORDER_TEMPLATE_ID int4, POSTP_NUM int4, SELECTED_NUM int4, DELETE_NUM int4, FINAL_NUM int4, FINAL_ORIGINAL_NUM int4, ORIGINAL_NUM int4, ORIGINAL_RAW_NUM int4, POSTP_ORIGINAL_NUM int4, REALTIME_NUM int4, THUMB_NUM int4, VBOX_OLD_CODE text, INVITATION_CODE text, VBOX_OLD_CODE_FIX text, SETTLE_WAY int4, intervalCount int4);

CREATE TABLE v_tbl_user_expense_record (id int4 primary key, BALANCE_MONEY int8, CURR_MONEY int8, ORDER_ID int4, PERSON_ID int4, PREPAID_CODE text, REASON text, TRADE_CODE text, TRADE_MONEY int8, TYPE int4, UPDATE_TIME timestamp, USER_ID int4, CUSTOM_MANAGER_ID int4, EXPLAIN_COMMENT text, REL_CODE text, USER_EXPENSE_RECORD_ID int4, BANK_CODE text, SYS_USER_ID int4, GOODS_CODE text, CASH_BALANCE int8, COUPON_BALANCE int8, USER_SCENE int4, REFUND_BALANCE int8, GOODS_PURCHASE_ID int4);

CREATE TABLE v_tbl_user_order (ID int4 primary key, PACKAGE_ID int4, SHOOT_TIME_COST int4, ORDER_ID text, ORDER_PRICE numeric, ORDER_PAY_PRICE numeric, ORDER_STATE int4, PAY_STATE int4, ORDER_TIME timestamp, ACTIVITY_BUDGET_MIN int4, ACTIVITY_BUDGET_MAX int4, SHOOTING_JOIN_NUMBER float8, CITY_CODE text, SHOOTING_ADDRESS text, SHOOTING_TIME_STATE int4, SHOOTING_TIME_START timestamp, SHOOTING_TIME_END timestamp, REQUIREMENT_DESCRIPTION text, PHOTOGRAPHER_LEVEL_CODE text, PHOTOGRAPHER_NUMBER_CODE text, USER_ID int4, USER_NAME text, USER_COMPANY text, USER_PHONE text, USER_EMAIL text, WEIXIN_CONTENT text, UPDATE_TIME timestamp, SHOOTING_NAME text, PREPARE_PHOTOGRAPHER_PRICE numeric, ORDER_TYPE int4, NEGATIVE_REFINEMENT_NUMBER int4, WIFI_FLAG text, ACCOUNT_ID int4, ACTIVITY_DISCOUNT_CODE int4, ADD_SHOOT_TIME int4, COMPANY_PAY_PROFITS float8, OTHER_EXPENSES float8, OTHER_EXPENSES_DESCRIPTION text, SHOOTING_SCENE_CODE text, SUIT_PERSON_CODE text, PHOTOGRAPHER_SELECT_TYPE text, ADD_PAYMENT_CODE text, BILLING_RULES_CODE text, PAYMENT_TIME_END timestamp, USE_VPHOTOS_LOGO int4, USER_COMPANY_JC text, SUBACCOUNT_ID int4, RECOMMEND_ID int4, RECOMMEND_TYPE int4, AUTH_SELECTED_NUM int4, CAN_VIEW int4, USER_ID_PK int4, CAN_DOWNLOAD int4, CONTRACT_ID int4, CREATE_USER_ID int4, CUS_TAGLIB_TYPE_ID int4, CUSTOM_HEIGHT int4, CUSTOM_TYPE int4, CUSTOM_WIDTH int4, DEFAULT_DOWN_CODE text, digital_num text, DIGITAL_SCORE int4, DOUBLE_SIZE int4, DOUBLE_TYPE int4, NETWORK_SIZE int4, NETWORK_TYPE int4, OFFER_FOOD int4, OFFER_TRAFFIC int4, ORDER_END_TIME timestamp, ORDER_PAY_TIME timestamp, ORIGINAL_SIZE int4, ORIGINAL_TYPE int4, PHOTOGRAPHER_NUM int4, POSITION int4, POSITION_X int4, POSITION_Y int4, READY_SHOOT_TIME timestamp, REQUIREMENT_ID int4, SALE_PERSON int4, SCORE_COMMENT text, SCORE_DATE timestamp, SCORE_STATE int4, SELECT_NUM int4, SERVICE_SCORE int4, SHOOT_SCORE int4, THUMBNAIL_SIZE int4, VIEW_PRICE float8, salePerson text, FINAL_PROCESS int4, AUDIT_DIRECT int4, SELECT_POSTP_NUM int4, VPHOTOS_LOGO_POSITION int4, AUTO_HAND_SEQ int4, CUS_ID int4, CUS_SELECT int4, LIVE_VALIDATE int4, NEED_RAW int4, ORDER_LEVEL int4, PHOTO_SEQ int4, POSTP_NUM int4, SUCCESS_ID int4, TRANS_FIRST int4, TRANS_FIRST_NUM int4, TRANS_MODEL int4, ORDER_TEMPLATE_ID int4, TOUR_ORDER_ID int4, CAUTION text, JPG_ORIGINAL int4, NEW_ORDER_TYPE int4, MOVE_ORDER_ID int4, ORDER_CATEGORY int4, CUS_COM_SET_FINAL int4, CUS_COM_SET_POSTP int4, CUS_COM_SET_SELECTED int4, DOUBLE_COM_SET_FINAL int4, DOUBLE_COM_SET_POSTP int4, DOUBLE_COM_SET_SELECTED int4, DURE_MODEL_NEGATIVE int4, DURE_MODEL_ORI int4, DURE_MODEL_PREVIEW int4, EXPIRE_PERIOD int4, EXPIRE_SET int4, NET_WORK_COM_SET_FINAL int4, NET_WORK_COM_SET_POSTP int4, NET_WORK_COM_SET_SELECTED int4, ORIGINAL_COM_SET_FINAL int4, ORIGINAL_COM_SET_POSTP int4, ORIGINAL_COM_SET_SELECTED int4, THUMBNAIL_COM_SET_FINAL int4, THUMBNAIL_COM_SET_POSTP int4, THUMBNAIL_COM_SET_SELECTED int4, TRANS_FIRST_RAW int4, TRANS_FIRST_RAW_NUM int4, VBOX_SET_SELECTED int4, BASE_PRICE float8, USE_WATER_MARK int4, ORDER_PHOTO_BELONG int4, vboxCreate int4, VBOX_CREATE int4, SOURCE_TYPE int4, BACK_MONEY float8, DIGITAL_CONSUMPTION_NUM int4, PHOTOGRAPHER_CONSUMPTION_NUM int4, SELF_DIGITAL_NUM int4, SELF_PHOTOGRAPHER_NUM int4, REFUND_MONEY float8, CONFIRM_STATE int4, SETTLE_WAY int4, VER int4, BAD_DEBT_SOURCE_ORDER_ID int4, CRM_ORDER_ID text, CRM_ORDER_CODE text, CRM_LINKMAN_NAME text, CRM_LINKMAN_PHONE text, CRM_CONTRACT_REMAKE text, CRM_IMPORTANT_ORDER_MARK int4, CRM_IS_AUTH_VPHOTO int4, CRM_GET_ALBUM_TYPE int4, CRM_IS_PIC_WATERMARK int4, CRM_IS_ALBUM_COVER int4, CRM_IS_ALBUM_BANNER int4, CRM_REMARK text, GROUP_BUY_ID int4);

CREATE TABLE v_tbl_user_order_deal (ID int4 primary key, DEAL_TYPE int4, DESC_INFO text, ORDER_ID int4, SUGGEST_ID int4, SYS_USER_ID int4, SYS_USER_NAME text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_user_order_pay_state (ID int4 primary key, STATE_CODE int4, STATE_VALUE text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_user_pay (id int4 primary key, AVAILABLE_MONEY int8, PAY_PASSWORD text, PAY_STATUS int4, UPDATE_TIME timestamp, USER_ID int4, PAID_MONEY int8, AVAILABLE_COUPON_MONEY int8, REFUND_MONEY int8);

```



```
CREATE TABLE v_tbl_digital_balance_detail (ID int4 primary key, BALANCE_CODE text, BALANCE_STATE int4, BILL_WATER_ID int4, CREATE_TIME timestamp, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_assign_photographer_log (ID int4 primary key, CREATE_TIME timestamp, OPERATOR_FROM text, OPERATOR_TYPE text, OPERATOR int4, ORDER_ID text, ORDER_STATUS int4, PHOTOGRAPHER_ACCEPT_ORDER_STATES text, PHOTOGRAPHER_AMOUNT float8, PHOTOGRAPHER_ID int4, PHOTOGRAPHER_NAME text, PHOTOGRAPHER_STATUS int4, REMARK text, SCENE text, SHOOTING_END_TIME timestamp, SHOOTING_NAME text, SHOOTING_START_TIME timestamp, VBOX_CODE text);

CREATE TABLE v_tbl_billsheet (ID int4 primary key, BILL_ITEM int4, BILL_TYPE int4, CONFIRM_USER_ID int4, CONFIRM_USER_NAME text, CONFIRM_USER_TYPE int4, CREATE_TIME timestamp, CREATE_USER_ID int4, CREATE_USER_NAME text, CREATE_USER_TYPE int4, DATA_ENABLE int4, INTERVAL_ID int4, IS_TAKEIN int4, BILL_NAME text, ITEM_NUMBER int4, ITEM_PRICE float8, ITEM_TYPE int4, ORDER_ID int4, RATE float8, SOURCEINTERVAL_ID int4, TOTAL_PRICE float8, UPDATE_TIME timestamp, VERSION int4);

CREATE TABLE v_tbl_city (ID int4 primary key, COUNTRY_CODE text, COUNTRY_NAME text, CITY_CODE text, CITY_SHORT text, CITY_NAME text, SHOW_FLAG int4, SERVICE_FLAG int4, UPDATE_TIME timestamp, CITY_POSTCODE text, SEQUENCE int4, CITY_ID int8, COUNTY_ID int8, IS_SHOW int4, PROVICE_ID int4, REMARK text, EN_CITY_NAME text);

CREATE TABLE v_tbl_client_user_like (id int4 primary key, user_id int4, photo_id text, create_time timestamp, album_id int4, order_id int4, photo_name text, TYPE int4);

CREATE TABLE v_tbl_digital_amount_billwater (ID int4 primary key, AFTER_MONEY float8, AMOUNT_ID int4, AMOUNT_MONEY float8, BEFOR_MONEY float8, CREATE_TIME timestamp, DIGITAL_ID text, NOTE text, ORDER_ID int4, PAY_TYPE int4, BALANCE_STATE int4, BILL_CODE text, REMARK text, USER_ID int4);

CREATE TABLE v_tbl_digital_balance (ID int4 primary key, AMOUNT_MONEY float8, BALANCE_CODE text, BALANCE_REASON text, BALANCE_STATE int4, BALANCE_TIME timestamp, BALANCE_USER int4, BALANCE_WAY text, DIGITAL_CODE text, FOR_PAY float8, MONTH_PAY float8, MONTH_PERIOD text, PARENT_ID int4, PRE_BALANCE_TIME timestamp, PRE_BALANCE_USER int4, REMARK text, RESET_BALANCE_TIME timestamp, RESET_BALANCE_USER int4, SHOULD_PAY float8, TOTAL_SUM float8);

CREATE TABLE v_tbl_digital_company (id int4 primary key, COMPANY_ID text, DIGITAL_ID text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_digital_number (ID int4 primary key, NUMBER_CODE text, NUMBER_ITEM int4, NUMBER_VALUE text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_digital_photographer (ID int4 primary key, DIGITAL_ORDER_ID int4, PHOTOGRAPHER_ID int4, ORDER_ID int4, ACTIVE int4, INTERVAL_ID int4);

CREATE TABLE v_tbl_digital_service_city (ID int4 primary key, CITY_CODE text, CITY_NAME text, DIGITAL_ID text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_order_interval (ID int4 primary key, CREATE_TIME timestamp, DATA_ENABLE int4, DURATION float8, END_TIME timestamp, INTERVAL_NAME text, ORDER_ID int4, PRE_END_TIME timestamp, PRE_START_TIME timestamp, START_TIME timestamp, UPDATE_TIME timestamp, VERSION int4);

CREATE TABLE v_tbl_package (ID int4 primary key, PRODUCT_CODE text, PACKAGE_NAME text, PACKAGE_PRICE numeric, PACKAGE_PRICE_SHOW text, PACKAGE_PRICE_DESCRIPTION text, OUTTIME_CODE text, CHARGING_METHOD_CODE int4, SHOOTING_LENGTH int4, SHOOTING_LENGTH_SHOW text, PHOTOGRAPHER_NUMBER_CODE text, PHOTOGRAPHER_NUMBER_ADVICE_CODE text, PHOTOGRAPHER_LEVEL_CODE text, NEGATIVE_NUMBER int4, NEGATIVE_NUMBER_SHOW text, NEGATIVE_REFINEMENT_CODE text, SUIT_PERSON_NUMBER_CODE text, SHOOTING_ENVIRONMENT_CODE text, SERVICE_DESCRIPTION text, PACKAGE_STATE int4, PACKAGE_ORDER_BY int4, PACKAGE_DESCRIPTION text, UPDATE_TIME timestamp, PACKAGE_ID text, ACCOUNT_ID int4, PACKAGE_TYPE int4, NEGATIVE_REFINEMENT int4, NEGATIVE_REFINEMENT_SHOW text, SHOOTING_TIME_CODE text, SHOW_TYPE int4, DEFAULT_DOWN_CODE text, DOUBLE_SIZE int4, DOUBLE_TYPE int4, ICON1_URL text, ICON2_URL text, NETWORK_SIZE int4, NETWORK_TYPE int4, ORIGINAL_SIZE int4, ORIGINAL_TYPE int4, SEQ int4, THUMBNAIL_SIZE int4, THUMBNAIL_URL text, PACKAGE_SCENE text);

CREATE TABLE v_tbl_photo_album (ID int4 primary key, name text, userId int4, photoAlbumType int4, orderId int4, cover text, url text, publicity int4, delType int4, createTime timestamp, bucket text, path text, status int4, documentTitle text, footTitle text, headTitle text, qrCode text, weChatId text, weChatPrefix text, logoBucket text, logoKey text, customUrl text, customWeChatId text, photoSizes text, updateVersion int4, needori int4, orinum int8, oristatus int4, CANSHARE text, orifinalstatus int4, wechatMd5 text, zipstatues int4, qrCodeType int4, bannerType int4, bannerUrl text, wechatCoverType int4, wechatCoverUrl text, needPowered int4, needRequirement int4, directType int4, headContent text, sortBy int4, photoDown int4, photoUp int4, photoDownHeight int4, photoDownKey text, photoDownWidth int4, photoUpHeight int4, photoUpKey text, photoUpWidth int4, photoKey text, BUY_NUM int4, TOTAL_BUY_NUM int4, USE_FREE_NUM int4, DELETE_NUM int4, FINAL_NUM int4, FINAL_ORIGINAL_NUM int4, ORIGINAL_NUM int4, ORIGINAL_RAW_NUM int4, POSTP_NUM int4, POSTP_ORIGINAL_NUM int4, REALTIME_NUM int4, THUMB_NUM int4, UNLOCK_NUM int4, BANNER_LINK text, COVER_LINK text, COVER_SHOW_SET int4, FLOW_ADD_NUM int4, FLOW_SET int4, GUIDE_BUTTON_CONTENT text, GUIDE_SET int4, PREBACK_KEY text, PREBACK_SET int4, PREPHONE text, PREPHONE_SET int4, PREPHONE_TYPE int4, TRADE_TYPE int4, USE_YHQ int4, YHQ_BUTTON_CONTENT text, YHQ_TEMPLATE_ID int4, COLUMN_SHOW_SET int4, SORT_SHOW_SET int4, photoAlbumQrcodeKey text, CUSTOM_DEL_NUM int4, FINAL_DEL_NUM int4, MUILTIPLE_REPLY_ID int4, POSTP_DEL_NUM int4, REALTIME_DEL_NUM int4, REFINE_DEL_NUM int4, slCode text, THUMB_DEL_NUM int4, aboutUs int4, activityInfoMasterSwitch int4, activityMoreSence int4, activityProcess int4, albumBaseMasterSwitch int4, albumColor int4, albumPerMasterSwitch int4, albumValueMasterSwitch int4, bannerNum int4, baseMoreScence int4, columnShowSetButton int4, copyrightMasterSwitch int4, coverAtutoSkipTime int4, coverAutoSkip int4, coverSet int4, documentTitleRemark text, documentTitleSysUserId int4, documentTitleUpdateTime text, entranceButtonKey text, entranceButtonSet int4, guestInfo int4, headContentRemark text, headContentSysUserId int4, headContentUpdateTime text, lastPhotoDefaultHeight int4, lastPhotoDefaultKey text, lastPhotoDefaultWidth int4, lastPhotoHeight int4, lastPhotoKey text, lastPhotoSet int4, lastPhotoWidth int4, masterParty int4, pasterAutoSkip int4, pasterAutoSkipTime int4, pasterContentKey text, pasterContentSet int4, pasterSet int4, photoPurchase int4, photoRanking int4, poweredRemark text, poweredSysUserId int4, poweredUpdateTime text, puzzleSet int4, seeDirectPass text, shootingParty int4, sortShowSetButton int4, sparePhotographer int4, sponsor int4, tagSelected int4, WAIT_SET int4, wechatCoverKey text, YHQ_BUTTON_SET int4, headTitleFontSize int4, smallTime int4, showDigital int4, showPhotographer int4, SHOWPVUV int4, digitalPrefix text, photographerPrefix text, DEFAULT_BANNER_CODE text, footKey text, footType int4, poweredContent text, poweredPosition int4, sharePicButton int4, sharePicCode text, sharePicKey text, LANGUAGE text, FLOW_BUTTON int4, FLOW_URL text, SHARE_SET_URL text, SHOW_LEVEL int4, LIKE_STATISTICS int4, LIKE_HEADPIC int4, ALBUM_QR_TYPE int4, ALBUYM_QR_CUS_KEY text, ALBUM_QR_FIRST_TITLE text, ALBUM_QR_SEC_TITLE text, CLIENT_USER_ID int4, ACTIVITY_ADDRESS text, ACTIVITY_CONTENT text, ACTIVITY_FONT_SIZE int4, ACTIVITY_STATUS int4, ACTIVITY_TIME text, ACTIVITY_TITLE text, ALBUM_QR_CUS_FIRST_TITLE text, ALBUM_QR_CUS_SEC_TITLE text, BANNER_STATUS int4, SUB_HEAD_TITLE text, VERSION text, MINI_APP_SHOW_LEVEL int4, defaultCoverCode text, DOCUMENT_MAIN_TITLE text, ACTIVITY_CHOICE_SWITCH int4, COVER_STYLE int4, SERVICE_SUPPORT_SWITCH int4, VPHOTO_BRAND_SWITCH int4, WECHAT_COVER_THUMB int4, WECHAT_HORIZONTAL_COVER_KEY text, WECHAT_HORIZONTAL_COVER_THUMB int4, FACE_CLUSTER_SWITCH int4, SCENE_CODE text, QR_SWITCH int4);

CREATE TABLE v_tbl_photo_album_user (id int4 primary key, ALBUM_ID int4, CREATE_DATE timestamp, CREATE_USER_ID int4, USER_NAME text, USER_PHONE text);

CREATE TABLE v_tbl_photoerdigital_source (ID int4 primary key, CREATE_TIME timestamp, DATA_ENABLE int4, INTERVAL_ID int4, ORDER_ID int4, REAL_END_TIME timestamp, REAL_START_TIME timestamp, SINGLE_STATUS int4, SOURCE_INTERVAL_ID int4, UPDATE_TIME timestamp, USER_ID text, USER_IDS int4, USER_TYPE int4);

CREATE TABLE v_tbl_photographer_balance (ID int4 primary key, AMOUNT_MONEY float8, BALANCE_CODE text, BALANCE_REASON text, BALANCE_STATE int4, BALANCE_TIME timestamp, BALANCE_USER int4, BALANCE_WAY text, FOR_PAY float8, MONTH_PAY float8, MONTH_PERIOD text, PARENT_ID int4, PHOTOGRAPHER_CODE text, PRE_BALANCE_TIME timestamp, PRE_BALANCE_USER int4, REMARK text, RESET_BALANCE_TIME timestamp, RESET_BALANCE_USER int4, SHOULD_PAY float8, TOTAL_SUM float8, REFUND_MONEY float8, TYPE int4);

CREATE TABLE v_tbl_photographer_balance_detail (ID int4 primary key, BALANCE_CODE text, BALANCE_STATE int4, BILL_WATER_ID int4, CREATE_TIME timestamp, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_photographer_bill_water (ID int4 primary key, PHOTOGRAPHER_ID text, CREATE_TIME timestamp, CONTENT text, ORDER_ID int4, BEFOR_MONEY numeric, AMOUNT_MONEY numeric, AFTER_MONEY numeric, SETTLEMENT_FLAG int4, TYPE_FLAG int4, NOTE text, AMOUNT_NAME text, AMOUNT_BANK text, AMOUNT_BRANCH text, AMOUNT_ACCOUNT text, AMOUNT_ID int4, BALANCE_STATE int4, BILL_CODE text, PAY_TYPE int4, REMARK text, USER_ID int4, REFUND_MONEY float8, TYPE int4, REFUND_TO_CASH_OPERATOR int4, REFUND_TO_CASH_TIME timestamp, WATER_TYPE int4);

CREATE TABLE v_tbl_photographer_level (ID int4 primary key, LEVEL_CODE text, LEVEL_VALUE text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_photographer_number (ID int4 primary key, NUMBER_CODE text, NUMBER_VALUE text, UPDATE_TIME timestamp, NUMBER_ITEM int4);

CREATE TABLE v_tbl_photographer_order_day (id int4 primary key, DELETE_NUM int4, FINAL_NUM int4, FINAL_ORIGINAL_NUM int4, FIRST_FS_PHOTO_NAME text, FIRST_FS_PHOTO_TIME timestamp, FIRST_PHOTO_NAME text, FIRST_PHOTO_TIME timestamp, LAST_FS_PHOTO_NAME text, LAST_FS_PHOTO_TIME timestamp, LAST_PHOTO_NAME text, LAST_PHOTO_TIME timestamp, ORDER_ID int4, ORIGINAL_NUM int4, ORIGINAL_RAW_NUM int4, PHOTO_DATE timestamp, PHOTOGRAPHER_ID int4, POSTP_ORIGINAL_NUM int4, REALTIME_NUM int4, THUMB_NUM int4);

CREATE TABLE v_tbl_photographer_order_state (ID int4 primary key, STATE_CODE int4, STATE_VALUE text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_photographer_service_city (ID int4 primary key, PHOTOGRAPHER_ID text, CITY_CODE text, CITY_NAME text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_photographer_settle_price (ID int4 primary key, ADJUST_PRICE float8, ADJUST_TYPE_ID int4, adjustTypeName text, CREATE_TIME timestamp, ORDER_ID int4, ORDER_STATUS int4, REMARK text, SYS_USER_ID int4, SYS_USER_NAME text);

CREATE TABLE v_tbl_photographer_vbox (id int4 primary key, PHOTOGRAPHER_ID text, STATUS int4, UPDATE_TIME timestamp, VBOX_CODE text, VBOX_IP text);

CREATE TABLE v_tbl_source_interval (ID int4 primary key, CREATE_TIME timestamp, DATA_ENABLE int4, INTERVAL_ID int4, LEVEL text, MACHINE_NUMBER int4, ORDER_ID int4, PRE_SOURCE_NUMBER int4, PRICE float8, PURCHASE_ID int4, SETTLEMENT int4, SOURCE_NUMBER int4, SOURCE_TYPE int4, TYPE_ID int4, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_sysuser_dept (id int4 primary key, deptLevel int4, deptName text, isShow int4, parentId int4);

CREATE TABLE v_tbl_user_order_camerastand (ID int4 primary key, ORDER_ID int4, PHOTOGRAPHER_NUM int4, PRICE int4, TYPE_ID int4);

CREATE TABLE v_tbl_user_order_dept (id int4 primary key, orderId int4, sysuserDeptId int4);

CREATE TABLE v_tbl_user_order_state (ID int4 primary key, STATE_CODE int4, STATE_VALUE text, UPDATE_TIME timestamp);

CREATE TABLE v_tbl_user_order_suggest (ID int4 primary key, ERROR_TYPE int4, ORDER_ID int4, PHOTOGRAPHER_CODE text, SUGGEST_NAME text, SUGGEST_TYPE int4);
```

