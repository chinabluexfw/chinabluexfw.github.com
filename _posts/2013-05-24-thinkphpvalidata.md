---
layout : post
category : lessons
tags :  thinkphp
title :  thinkphp 让数据验证变得简单

---


1 数据验证类 ValiDataUtil
	提供以下验证方法

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

	


	
	

