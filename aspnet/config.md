# Overview of ASP.NET MVC ConfigurationSection
```csharp
public class MyConfig : IConfigurationSectionHandler
{
    public object Create(object parent, object configContext, XmlNode section)
    {
        MyConfig config = new MyConfig();
        var redisNode = section.SelectSingleNode("RedisCaching");
        config.RedisCachingEnabled = GetBool(redisNode, "Enabled");
        config.RedisCachingConnectionString = GetString(redisNode, "ConnectionString");
        return config;
    }

    private bool GetBool(XmlNode node, string attrName)
    {
        return SetByXElement(node, attrName, Convert.ToBoolean);
    }
    private string GetString(XmlNode node, string attrName)
    {
        return SetByXElement(node, attrName, Convert.ToString);
    }

    private T SetByXElement<T>(XmlNode node, string attrName, Func<object, T> converter)
    {
        if (node == null || node.Attributes == null) return default(T);
        var attr = node.Attributes[attrName];
        var attrVal = attr?.Value;
        return converter(attrVal);

    }

    public bool RedisCachingEnabled { get; private set; }
    public string RedisCachingConnectionString { get; private set; }

}
```
