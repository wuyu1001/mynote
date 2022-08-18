# superset 本地安装部署

* pip install superset 
* pip install flask-appbuilder 安装flask用户权限角色管理
* 安装superset各种依赖包
* superset db upgrade 更新superset 数据库
* 导入环境变量export FLASK_APP=superset
* flask fab create-admin 创建登录管理员账号
* superset load_examples 载入案例数据
* superset init 初始化角色和权限
* superset runserver 注意：Error: No such command “runserver”，用superset run -p 8088启动