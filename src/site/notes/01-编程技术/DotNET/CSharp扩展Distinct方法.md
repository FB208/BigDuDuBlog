---
{"dg-publish":true,"permalink":"/01-编程技术/DotNET/CSharp扩展Distinct方法/","dgPassFrontmatter":true,"updated":"2023-11-09T17:12:27.000+08:00"}
---

#dotnet #csharp

C# list集合去重，默认只支持简单类型如 ``List<string>`` ,通过以下方法扩展Distinct，使之可以比对 ``List<T>`` 中的某几列，并去重


``` C#
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
namespace WebApplication1
{
    public partial class quDiaoChongFu : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            List<test> tlist = new List<test>();
            for (int i = 0; i < 99; i++)
            {
                for (int j = 0; j < 99; j++)
                {
                    test t = new test();
                    t.id = j;
                    t.mm = "asdjfosadijf jsdofijsoijfoisjdfoi asjdfo sjdo fijsdoif ";
                    t.qq = "asdfsadf ";
                    t.ww = "sdfgfyrt";
                    tlist.Add(t);
                }
            }
            DateTime d1 = DateTime.Now;
            tlist = tlist.Distinct(m => m.id).ToList();
            double d2 = (DateTime.Now - d1).TotalMilliseconds;
            Response.Write(d2 + "<br/>");
            Response.Write(tlist.Count);
        }
        public class test
        {
            public int id { get; set; }
            public string mm { get; set; }
            public string qq { get; set; }
            public string ww { get; set; }
            public string ee { get; set; }
            public string rr { get; set; }
            public string tt { get; set; }
            public string yy { get; set; }
            public string uu { get; set; }
            public string ii { get; set; }
        }
    }
    public class CommonEqualityComparer<T, V> : IEqualityComparer<T>
    {
        private Func<T, V> keySelector;
        public CommonEqualityComparer(Func<T, V> keySelector)
        {
            this.keySelector = keySelector;
        }
        public bool Equals(T x, T y)
        {
            return EqualityComparer<V>.Default.Equals(keySelector(x), keySelector(y));
        }
        public int GetHashCode(T obj)
        {
            return EqualityComparer<V>.Default.GetHashCode(keySelector(obj));
        }
    }
    public static class DistinctExtensions
    {
        public static IEnumerable<T> Distinct<T, V>(this IEnumerable<T> source, Func<T, V> keySelector)
        {
            return source.Distinct(new CommonEqualityComparer<T, V>(keySelector));
        }
    }  
}
```