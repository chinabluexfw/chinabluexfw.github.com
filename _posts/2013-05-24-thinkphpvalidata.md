---
layout : post
category : lessons
tags :  thinkphp
title :  thinkphp 让数据验证变得简单

---


1 数据验证类 ValiDataUtil提供以下验证方法

	 /**
	 * 根据验证规则验证数据字段.
	 * @param Array $data key/value 数组
	 * @param Array  $rulelist 验证表达式列表
	 * @param Boolean  $onerror 验证失败后，是否继续验证
	 * array('验证字段名','验证规则','提示信息！','验证条件','附加规则')
	 * 验证条件：
	 * 1 必须验证
	 * 2 值不为空的时候验证
	 * @return Boolean
	 */
	public function ValidData($data,$rulelist,$onerror = false)
	{
	}
	
	/**
	 * 根据验证规则验证数据.
	 * @param Array $data key/value 数组
	 * @param Array  $rule 验证表达式  
	 * array('验证字段名','验证规则','提示信息！','验证条件','附加规则')
	 * 验证条件：0 存在字段就验证（默认） 
	 * 1 必须验证 
	 * 2 值不为空的时候验证
	 * @return Boolean
	 */
	public function Valid($data,$rule) 
	{
	}
	
	/**

     * 验证数据值 支持 in between equal length regex expire ip_allow ip_deny.
     * @param String $value 验证数据
     * @param Mixed  $rule 验证表达式
     * @param String $type 验证方式 默认为正则验证
     * @return Boolean
     */
    public function Check($value,$rule,$type='regex')
	{
	
	}

	

2 直接验证数据

	$_rule = array(
			array('UserName','require','用户名必填！',1),
			array('Email','email','邮箱格式错误！',1),
			array('Password','password','密码格式错误！',1),
			array('Password2','Password','确认密码不一致！',1,'confirm'),
			array('RoleId','integer','角色必选！',1),
			array('UserStatus','0,1','状态必选！',1,'in'),
	)
	import('@.Util.ValiDataUtil');
    $u_valiUtil = new ValiDataUtil();
	if(!$u_valiUtil->ValidData($_REQUEST,$_rule)) {
		$u_valiUtil->error();
	}

4 拦截器验证数据

	创建数据拦截器
	class ValiDataBehavior extends Behavior 
	{

		
		public function run(&$param)
		{
			//计算拦截器名称
			$_behaviorName = str_replace('Behavior','',get_class($this));
			$_config = Ap::GetInterceptorConfig($_behaviorName);
			
			if (false !== $_config) {
				$_valiData = Ap::GetStatic('VALID_DATA.'.MODULE_NAME.'.'.ACTION_NAME);
				if (!empty($_valiData)) {
					import('@.Util.ValiDataUtil');
					$u_valiUtil = new ValiDataUtil();
					$_error = $u_valiUtil->ValidData($_REQUEST,$_valiData,true);
					if (!$_error) {
						$a_empty = A('Empty');
						$a_empty->ShowMsg('error',$u_valiUtil->error("<br/>"));
					}
				}
			}
		}

	}
	
	创建Action 对应的验证规则，创建User.php 规则文件，内容如下
	return array(
		//array('验证字段名','验证规则','提示信息！','验证条件','附加规则')
		'User'=> array(
			'doadd'=> array(
				array('UserName','require','用户名必填！',1),
				array('Email','email','邮箱格式错误！',1),
				array('Password','password','密码格式错误！',1),
				array('Password2','Password','确认密码不一致！',1,'confirm'),
				array('RoleId','integer','角色必选！',1),
				array('UserStatus','0,1','状态必选！',1,'in'),
			),
			'doedit'=> array(
				array('Email','email','邮箱格式错误！',1),
				array('Password','password','密码格式错误！',2),
				array('Password2','Password','确认密码不一致！',2,'confirm'),
				array('RoleId','integer','角色必选！',1),
				array('UserStatus','0,1','状态必选！',1,'in'),
				array('lian',array(
						array('name','integer','联系方式必填！',1),
						array('sex','integer','性别设置！',1),
					),'',1,'array'
				)	
			)
				
		)	
		
	);
	
	配置拦截器运行
	
	

