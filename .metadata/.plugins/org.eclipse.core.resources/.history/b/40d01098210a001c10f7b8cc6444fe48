package cn.edu.zucc.personplan.comtrol.example;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.Date;

import cn.edu.zucc.personplan.itf.IUserManager;
import cn.edu.zucc.personplan.model.BeanUser;
import cn.edu.zucc.personplan.util.BaseException;
import cn.edu.zucc.personplan.util.BusinessException;
import cn.edu.zucc.personplan.util.DBUtil;
//import sun.tools.jconsole.ConnectDialog;
import cn.edu.zucc.personplan.util.DbException;

public class ExampleUserManager implements IUserManager {

	@Override
	// 注册方法,判断注册信息是否符合要求,并返回user类
	public BeanUser reg(String userid, String pwd,String pwd2) throws BaseException {
		// TODO Auto-generated method stub
		//逻辑判断,报错处理
		if(userid==null || "".equals(userid)) throw new BusinessException("账号不能为空");
		if(pwd==null || "".equals(pwd)) throw new BusinessException("密码不能为空");
		//TODO 二次密码为空?
		if(!pwd.equals(pwd2)) throw new BusinessException("两次密码不一致");
		//连接数据库,判断新增用户是否存在,如果存在报错表示用户已存在,不存在则数据库内插入新用户
		Connection connection =null;
		try {
			connection=DBUtil.getConnection();
			String sqlString ="select user_id from tbl_user where user_id = ?";
			//准备执行的语句
			java.sql.PreparedStatement pStatement = connection.prepareStatement(sqlString);
			pStatement.setString(1, userid);
			//执行语句
			java.sql.ResultSet rSet = pStatement.executeQuery();
			if(rSet.next()) {
				rSet.close();
				pStatement.close();
				throw new BusinessException("用户已存在");
			}
			//关闭前面的数据库执行语句,并新建插入语句,进行执行
			rSet.close();
			pStatement.close();
			//
			sqlString ="insert into tbl_user(user_id, user_pwd,register_time) values(?,?,?)";
			pStatement =connection.prepareStatement(sqlString);
			pStatement.setString(1, userid);
			pStatement.setString(2, pwd);
			pStatement.setTimestamp(3, new java.sql.Timestamp(System.currentTimeMillis()));
			pStatement.execute();
			//执行结束
			BeanUser user =new BeanUser();
			user.setUser_id(userid);
			user.setRegister_time(new Date());
			return user;
			
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			throw new DbException(e);
		}finally {
			if(connection!=null) {
				try {
					connection.close();
				} catch (Exception e2) {
					// TODO: handle exception
					e2.printStackTrace();
				}
			}
		}
	}



	@Override
	public BeanUser login(String userid, String pwd) throws BaseException {

		if(userid==null || "".equals(userid)) throw new BusinessException("账号不能为空");
		if(pwd==null || "".equals(pwd)) throw new BusinessException("密码不能为空");
		Connection connection =null;
		try {
			
			connection=DBUtil.getConnection();
			String sqlString ="select register_time from tbl_user where user_id = ? and user_pwd = ?";
			//准备执行的语句
			java.sql.PreparedStatement pStatement = connection.prepareStatement(sqlString);
			pStatement.setString(1, userid);
			pStatement.setString(2, pwd);
			//执行语句
			java.sql.ResultSet rSet = pStatement.executeQuery();

			BeanUser user =null;
			if(rSet.next()) {
					//密码一致,返回用户信息
					user = new BeanUser();
					user.setUser_id(userid);
					user.setCurrentLoginUser(user);
					// 时间用时间戳获取吗?
					//
					user.setRegister_time(rSet.getTimestamp(1));

					rSet.close();
					pStatement.close();
					return user;
			}else {
				throw new BusinessException("用户不存在或者密码错误");
			}
			//关闭前面的数据库执行语句,并新建插入语句,进行执行
			
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			throw new DbException(e);
		}finally {
			if(connection!=null) {
				try {
					connection.close();
				} catch (Exception e2) {
					// TODO: handle exception
					e2.printStackTrace();
				}
			}
		}
	}


	@Override
	public void changePwd(BeanUser user, String oldPwd, String newPwd,
			String newPwd2) throws BaseException {
		// TODO Auto-generated method stub
		if(oldPwd==null || "".equals(oldPwd)||newPwd==null ||"".equals(newPwd)||newPwd2==null || "".equals(newPwd2)) 
				throw new BusinessException("新旧密码均不能为空");
		if(oldPwd.equals(newPwd)) throw new BusinessException("新旧密码不能一样");
		if(!newPwd.equals(newPwd2)) throw new BusinessException("两次密码输入不一致");
	    Connection connection=null;
	    try {
			connection=DBUtil.getConnection();
			String sql="update tbl_user set user_pwd = ? where user_id = ?";
			java.sql.PreparedStatement pst=connection.prepareStatement(sql);
			String userid = user.getUser_id();
			pst.setString(2,userid);
			pst.setString(1,newPwd);
			pst.execute();
		}catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			throw new BaseException("密码修改失败");
		}finally {
			if(connection!=null) {
				try {
					connection.close();
				} catch (Exception e2) {
					// TODO: handle exception
					e2.printStackTrace();
				}
			}
		}
		
	}

}
