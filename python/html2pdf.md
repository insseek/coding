# 网页转PDF(后端python)

### 实现方式

前端调用接口后， 后端调用工具Prince，将HTML页面转换为PDF， 并返回给前端

### 工具

Prince：https://www.princexml.com/

Prince可以读取HTML页面，并把HTML页面转换为PDF

PyPDF2：处理pdf文件的Python库

### 准备工作

1、服务器安装Prince

2、打印版需要特殊样式，要在html页面写一个打印样式

```
示例：
<link type="text/css" href="/reports/styles/report-pdf.scss" media="print"/>
Chrome浏览器，右键点击打印可预览打印后的样式
```

3、前端调用生成pdf的接口后代码中，后端调用Prince工具，读取相关url页面的HTML生成PDF文件，并返回给前端

python代码示例：

```python
#接口
def pdf(request, uid):
    #获取报告的pdf是否存在， 如果不存在生成PDF
    if not os.path.isfile(pdf_gen.report_path(uid)):
        pdf_gen.gen(request, uid)
    
    #pdf文件返回给前端
    file = open(pdf_gen.report_path(uid), 'rb')
    report = Report.objects.get(uid=uid)
    filename = report.title + report.version
    response = HttpResponse(file, content_type='application/pdf')
    response['Content-Disposition'] = 'attachment; filename={}.pdf'.format(
        filename)
    return response

#生成PDF的方法
import os.path
import subprocess

from django.urls import reverse
from django.conf import settings
from PyPDF2 import PdfFileReader, PdfFileWriter

# 生成PDF文件的服务器路径
def report_path(report_id):
    return '{}.pdf'.format(settings.MEDIA_ROOT + report_id)

# 获取报告页面的地址 比如：https://chilunyc.com/reports/r1910152EivqDmzRT6KalB0/
def report_url(request, report_id):
    if request.is_secure():
        protocol = 'https'
    else:
        protocol = 'http'
    report_url = reverse('reports:view', args=(report_id,))
    return protocol + "://" + request.get_host() + report_url

# 生成PDF的方法
def gen(request, report_id):
    
    #pdf临时文件路径1
    temp_path_1 = '{}temp-{}-1.pdf'.format(settings.MEDIA_ROOT, report_id)
    #pdf临时文件路径2
    temp_path_2 = '{}temp-{}-2.pdf'.format(settings.MEDIA_ROOT, report_id)
    
    #最终pdf输出路径
    out_path = report_path(report_id)
    
    # 页面的url
    url = report_url(request, report_id)
    #调用prince 读取html生成临时pdf1
    
    subprocess.call(['prince', url,
                     '--no-embed-fonts', '--no-artificial-fonts', '-o', temp_path_1])
                     
    #用PDF阅读器 临时pdf1 加页码               
    inputdu qfile = open(temp_path_1, 'rb')
    pdf = PdfFileReader(input_file)
    writer = PdfFileWriter()
    writer.addPage(pdf.getPage(0))
    writer.removeLinks()
    if pdf.getNumPages() > 0:
        for i in range(1, pdf.getNumPages()):
            writer.addPage(pdf.getPage(i))
            
    #加页码后的pdf1内容  输出到临时pdf2路径、 压缩后再输出到最终pdf路径  
    
    #是否压缩
    if settings.COMPRESS_PDF:
        out = open(temp_path_2, 'wb')
        writer.write(out)
        os.remove(temp_path_1)
        subprocess.call(['gs', '-sDEVICE=pdfwrite', '-dCompatibilityLevel=1.4', '-dPDFSETTINGS=/printer',
                         '-dNOPAUSE', '-dQUIET', '-dBATCH', '-dColorConversionStrategy=/LeaveColorUnchanged',
                         '-sOutputFile=' + out_path, temp_path_2])
        os.remove(temp_path_2)
    else:
        #加页码后的pdf1内容 输出到最终pdf路径
        out = open(out_path, 'wb')
        writer.write(out)
        os.remove(temp_path_1)

    return out_path
```