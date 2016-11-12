# rbac 
基于角色的数据库方式验证类

只支持thinkphp5版本

## RBAC 权限验证配置

``` php
return [
    // RBAC 权限验证
    'user_auth_on'            => true, // 是否开启认证
    'user_auth_type'          => 2, // 默认认证类型 1 - 登录认证 | 2 - 实时认证
    'user_auth_key'           => 'auth_id',    // 用户认证 SESSION 标记
    'admin_auth_key'          => 'administrator',
    'user_auth_model'         => '',    // 默认验证数据表模型
    'user_auth_gateway'       => '/login',    // 默认认证网关
    'not_auth_controller'     => 'Index',        // 默认无需认证控制器，多级控制器使用点语法，严格大小写，多个逗号隔开
    'require_auth_controller' => '',        // 默认需要认证控制器，多级控制器使用点语法，严格大小写，多个逗号隔开
    'not_auth_action'         => '',        // 默认无需认证方法，全部小写，多个逗号隔开
    'require_auth_action'     => '',        // 默认需要认证方法，全部小写，多个逗号隔开
    'common_auth_name'        => 'common',        // 公共授权控制器名称和方法名称
    'guest_auth_on'           => false,    // 是否开启游客授权访问
    'guest_auth_id'           => 0,     // 游客的用户ID

    'role_table'   => 'admin_role', // 角色表名称，不包含表前缀
    'user_table'   => 'admin_role_user', // 用户角色关系表名称，不包含表前缀
    'access_table' => 'admin_access', // 权限表名称，不包含表前缀
    'node_table'   => 'admin_node', // 节点表名称，不包含表前缀
];
```

## 相关表

```
CREATE TABLE IF NOT EXISTS `think_access` (
  `role_id` smallint(6) unsigned NOT NULL,
  `node_id` smallint(6) unsigned NOT NULL,
  `level` tinyint(1) NOT NULL,
  `module` varchar(50) DEFAULT NULL,
  KEY `groupId` (`role_id`),
  KEY `nodeId` (`node_id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

CREATE TABLE IF NOT EXISTS `think_node` (
  `id` smallint(6) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  `title` varchar(50) DEFAULT NULL,
  `status` tinyint(1) DEFAULT '0',
  `remark` varchar(255) DEFAULT NULL,
  `sort` smallint(6) unsigned DEFAULT NULL,
  `pid` smallint(6) unsigned NOT NULL,
  `level` tinyint(1) unsigned NOT NULL,
  PRIMARY KEY (`id`),
  KEY `level` (`level`),
  KEY `pid` (`pid`),
  KEY `status` (`status`),
  KEY `name` (`name`)
) ENGINE=MyISAM  DEFAULT CHARSET=utf8;

CREATE TABLE IF NOT EXISTS `think_role` (
  `id` smallint(6) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  `pid` smallint(6) DEFAULT NULL,
  `status` tinyint(1) unsigned DEFAULT NULL,
  `remark` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `pid` (`pid`),
  KEY `status` (`status`)
) ENGINE=MyISAM  DEFAULT CHARSET=utf8 ;

CREATE TABLE IF NOT EXISTS `think_role_user` (
  `role_id` mediumint(9) unsigned DEFAULT NULL,
  `user_id` char(32) DEFAULT NULL,
  KEY `group_id` (`role_id`),
  KEY `user_id` (`user_id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

