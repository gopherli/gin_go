# gin_go
learn_standard

# 路由中间件
- 剥离非业务代码
```
1. 涉及路由较多且这些路由都需要用到公共的代码时，需要考虑用中间件的方式剥离开来。
目的：保证非业务的需求在http请求处理前做一些事情，并且在响应完成之后做一些事情。
```

# 参数校验
1. validate库：gopkg.in/go-playground/validator.v9
2. 使用方法：
```
type Req struct {
    Username string   `validate:"gt=0"`
}
validate := validator.New()
err := validate.Struct(Req)
```

# 面向接口编程
```
type BusinessInstance interface {
    ValidateLogin()
    ValidateParams()
    AntispamCheck()
    GetPrice()
    CreateOrder()
    UpdateUserStatus()
    NotifyDownstreamSystems()
}

func BusinessProcess(bi BusinessInstance) {
    bi.ValidateLogin()
    bi.ValidateParams()
    bi.AntispamCheck()
    bi.GetPrice()
    bi.CreateOrder()
    bi.UpdateUserStatus()
    bi.NotifyDownstreamSystems()
}
```

# 表驱动开发
```
func entry() {
    var bi BusinessInstance
    switch businessType {
    case TravelBusiness:
        bi = travelorder.New()
    case MarketBusiness:
        bi = marketorder.New()
    default:
        return errors.New("not supported business")
    }
}

var businessInstanceMap = map[int]BusinessInstance {
    TravelBusiness : travelorder.New(),
    MarketBusiness : marketorder.New(),
}

func entry() {
    bi := businessInstanceMap[businessType]
}
```