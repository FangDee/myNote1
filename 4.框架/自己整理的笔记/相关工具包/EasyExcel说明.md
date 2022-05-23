#### EasyExcel说明

1. EasyExcel重写了poi对07版Excel的解析，降低了内存消耗，对模型转换进行了封装，然后写了下简单案例。
2. 完整代码地址在结尾！！

#### 第一步，导入maven依赖

```
<!-- 阿里开源框架EasyExcel -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>easyexcel</artifactId>
    <version>2.1.6</version>
</dependency>
<!-- 将excel转成csv格式的poi依赖 -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>4.1.0</version>
</dependency>
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>4.1.0</version>
</dependency>
```

#### 第二步，创建模型解析监听器，以下提供两种方式

##### 第一种，创建模型解析监听器类，ModelExcelListener

```
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;

import java.util.ArrayList;
import java.util.List;
/**
 * @Description: 模型解析监听器 -- 每解析一行会回调invoke()方法，整个excel解析结束会执行doAfterAllAnalysed()方法
 * @Author: jinhaoxun
 * @Date: 2020/1/14 15:51
 * @Version: 1.0.0
 */
public class ModelExcelListener<E> extends AnalysisEventListener<E> {

    private List<E> dataList = new ArrayList<E>();

    @Override
    public void invoke(E object, AnalysisContext context) {
        dataList.add(object);
    }

    @Override
    public void doAfterAllAnalysed(AnalysisContext context) {
    }

    public List<E> getDataList() {
        return dataList;
    }

    public void setDataList(List<E> dataList) {
        this.dataList = dataList;
    }
}
```

##### 第二种，创建StringList解析监听器类，StringExcelListener

```
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;

import java.util.ArrayList;
import java.util.List;

/**
 * @Description: StringList 解析监听器
 * @Author: jinhaoxun
 * @Date: 2020/1/15 11:31
 * @Version: 1.0.0
 */
public class StringExcelListener extends AnalysisEventListener {
    /**
     * 自定义用于暂时存储data
     * 可以通过实例获取该值
     */
    private List<List<String>> datas = new ArrayList<List<String>>();

    /**
     * 每解析一行都会回调invoke()方法
     *
     * @param object
     * @param context
     */
    @Override
    public void invoke(Object object, AnalysisContext context) {
        List<String> stringList= (List<String>) object;
        //数据存储到list，供批量处理，或后续自己业务逻辑处理。
        datas.add(stringList);
        //根据自己业务做处理
    }

    @Override
    public void doAfterAllAnalysed(AnalysisContext context) {
        //解析结束销毁不用的资源
        //注意不要调用datas.clear(),否则getDatas为null
    }

    public List<List<String>> getDatas() {
        return datas;
    }

    public void setDatas(List<List<String>> datas) {
        this.datas = datas;
    }
}
```

#### 第三步，分别创建三个模型类，ExcelModel，ExcelModel1，ExcelModel2

##### ExcelModel

```
import com.alibaba.excel.annotation.ExcelProperty;
import com.alibaba.excel.metadata.BaseRowModel;
import lombok.Data;

/**
 * @Description: 生成excel文件数据字段模板
 * @Author: jinhaoxun
 * @Date: 2020/1/14 15:45
 * @Version: 1.0.0
 */
@Data
public class ExcelModel extends BaseRowModel {

    public ExcelModel(){
    }

    public ExcelModel(String dateJuly, String onDuty, String offDuty, String overtime, String last){
        this.dateJuly = dateJuly;
        this.onDuty = onDuty;
        this.offDuty = offDuty;
        this.overtime = overtime;
        this.last = last;
    }

    @ExcelProperty(value = "日期", index = 0)
    private String dateJuly;
    @ExcelProperty(value = "上班时间", index = 1)
    private String onDuty;
    @ExcelProperty(value = "下班时间", index = 2)
    private String offDuty;
    @ExcelProperty(value = "加班时长", index = 3)
    private String overtime;
    @ExcelProperty(value = "备注", index = 4)
    private String last;

}
```

##### ExcelModel1

```
import com.alibaba.excel.annotation.ExcelProperty;
import com.alibaba.excel.metadata.BaseRowModel;
import lombok.Data;

/**
 * @Description: 生成excel文件数据字段模板
 * @Author: jinhaoxun
 * @Date: 2020/1/14 15:45
 * @Version: 1.0.0
 */
@Data
public class ExcelModel1 extends BaseRowModel {

    public ExcelModel1(){
    }

    public ExcelModel1(String dateJuly, String onDuty, String offDuty, String overtime, String last){
        this.dateJuly = dateJuly;
        this.onDuty = onDuty;
        this.offDuty = offDuty;
        this.overtime = overtime;
        this.last = last;
    }

    @ExcelProperty(value = "日期", index = 0)
    private String dateJuly;
    @ExcelProperty(value = "上班时间", index = 1)
    private String onDuty;
    @ExcelProperty(value = "下班时间", index = 2)
    private String offDuty;
    @ExcelProperty(value = "加班时长", index = 3)
    private String overtime;
    @ExcelProperty(value = "备注", index = 4)
    private String last;

}
```

##### ExcelModel2

```
import com.alibaba.excel.annotation.ExcelProperty;
import com.alibaba.excel.metadata.BaseRowModel;
import lombok.Data;

/**
 * @Description: 生成excel文件数据字段模板
 * @Author: jinhaoxun
 * @Date: 2020/1/14 15:45
 * @Version: 1.0.0
 */
@Data
public class ExcelModel2 extends BaseRowModel {

    public ExcelModel2(){
    }

    public ExcelModel2(String dateJuly, String onDuty, String offDuty, String overtime, String last){
        this.dateJuly = dateJuly;
        this.onDuty = onDuty;
        this.offDuty = offDuty;
        this.overtime = overtime;
        this.last = last;
    }

    @ExcelProperty(value = "日期", index = 0)
    private String dateJuly;
    @ExcelProperty(value = "上班时间", index = 1)
    private String onDuty;
    @ExcelProperty(value = "下班时间", index = 2)
    private String offDuty;
    @ExcelProperty(value = "加班时长", index = 3)
    private String overtime;
    @ExcelProperty(value = "备注", index = 4)
    private String last;

}
```

#### 第四步，创建工具类，DataConvertUtil，ExcelUtil，ExcelConvertCsvUtil

##### DataConvertUtil

```
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;

/**
 * @Description:
 * @Author: jinhaoxun
 * @Date: 2020/4/14 下午5:26
 * @Version: 1.0.0
 */
public class DataConvertUtil {

    /**
     * @Author: jinhaoxun
     * @Description: 将inputStream转byte[]
     * @param inputStream 输入流
     * @Date: 2020/1/16 21:43
     * @Return: byte[]
     * @Throws: Exception
     */
    public static byte[] inputStreamTobyte2(InputStream inputStream) throws IOException {
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        byte[] buff = new byte[100];
        int rc = 0;
        while ((rc = inputStream.read(buff, 0, 100)) > 0) {
            byteArrayOutputStream.write(buff, 0, rc);
        }
        return byteArrayOutputStream.toByteArray();
    }

    /**
     * @Author: jinhaoxun
     * @Description: 将byte[]转inputStream
     * @param bytes byte数组
     * @Date: 2020/1/16 21:43
     * @Return: InputStream
     * @Throws: Exception
     */
    public static InputStream byte2ToInputStream(byte[] bytes) {
        return new ByteArrayInputStream(bytes);
    }

}
```

##### ExcelUtil

```
import com.alibaba.excel.ExcelReader;
import com.alibaba.excel.ExcelWriter;
import com.alibaba.excel.metadata.BaseRowModel;
import com.alibaba.excel.metadata.Sheet;
import com.alibaba.excel.metadata.Table;
import com.alibaba.excel.support.ExcelTypeEnum;
import com.jinhaoxun.easyexcel.listener.ModelExcelListener;
import com.jinhaoxun.easyexcel.listener.StringExcelListener;

import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

/**
 * @Description: excel导入导出工具类
 * 1.支持按行导入字符串方式
 * 2.支持导入实体类映射
 * 3.支持按行导出字符串方式
 * 4.支持导出实体类映射
 * @Author: jinhaoxun
 * @Date: 2020/1/15 11:20
 * @Version: 1.0.0
 */
public class ExcelUtil {

    /**
     * @Author: jinhaoxun
     * @Description: 使用 StringList 来读取Excel
     * @param inputStream Excel的输入流
     * @param excelTypeEnum Excel的格式(XLS或XLSX)
     * @Date: 2020/1/16 21:40
     * @Return: java.util.List<java.util.List<java.lang.String>>
     * @Throws: Exception
     */
    public static List<List<String>> readExcel(InputStream inputStream, ExcelTypeEnum excelTypeEnum) throws Exception{
        StringExcelListener listener = new StringExcelListener();
        ExcelReader excelReader = new ExcelReader(inputStream, excelTypeEnum, null, listener);
        excelReader.read();
        return listener.getDatas();
    }

    /**
     * @Author: jinhaoxun
     * @Description: 使用模型来读取Excel
     * @param inputStream Excel的输入流
     * @param clazz 模型的类
     * @param excelTypeEnum Excel的格式(XLS或XLSX)
     * @Date: 2020/1/16 21:41
     * @Return: java.util.List<E>
     * @Throws: Exception
     */
    public static <E> List<E> readExcel(InputStream inputStream, Class<? extends BaseRowModel> clazz, ExcelTypeEnum excelTypeEnum) throws Exception {
        // 解析每行结果在listener中处理
        ModelExcelListener<E> listener = new ModelExcelListener<E>();
        ExcelReader excelReader = new ExcelReader(inputStream, excelTypeEnum, null, listener);
        //默认只有一列表头
        excelReader.read(new Sheet(1, 1, clazz));
        return listener.getDataList();
    }

    /**
     * @Author: jinhaoxun
     * @Description: 使用StringList来写入Excel，单sheet，单table
     * @param outputStream Excel的输出流
     * @param data 要写入的以StringList为单位的数据
     * @param table 配置Excel的表的属性
     * @param excelTypeEnum Excel的格式(XLS或XLSX)
     * @Date: 2020/1/16 21:42
     * @Return: void
     * @Throws: Exception
     */
    public static void writeExcel(OutputStream outputStream, List<List<String>> data, Table table, ExcelTypeEnum excelTypeEnum) throws Exception {
        /**
         * @Author: jinhaoxun
         * @Description:
         * @param outputStream
         * @param data
         * @param table
         * @param excelTypeEnum
         * @Date: 2020/1/16 21:42
         * @Return: void
         * @Throws:
         */
        //这里指定不需要表头，因为String通常表头已被包含在data里
        ExcelWriter writer = new ExcelWriter(outputStream, excelTypeEnum,false);
        //写第一个sheet, sheet1  数据全是List<String> 无模型映射关系,无表头
        Sheet sheet1 = new Sheet(0, 0);
        writer.write0(data, sheet1,table);
        writer.finish();
    }

    /**
     * @Author: jinhaoxun
     * @Description: 使用StringList来写入Excel，单sheet，单table（返回byte数组）
     * @param outputStream Excel的输出流
     * @param data 要写入的以StringList为单位的数据
     * @param table 配置Excel的表的属性
     * @param excelTypeEnum Excel的格式(XLS或XLSX)
     * @Date: 2020/1/16 21:43
     * @Return: byte[]
     * @Throws: Exception
     */
    public static byte[] writeExcel(ByteArrayOutputStream outputStream, List<List<String>> data, Table table, ExcelTypeEnum excelTypeEnum) throws Exception {
        //这里指定不需要表头，因为String通常表头已被包含在data里
        ExcelWriter writer = new ExcelWriter(outputStream, excelTypeEnum,false);
        //写第一个sheet, sheet1  数据全是List<String> 无模型映射关系,无表头
        Sheet sheet1 = new Sheet(0, 0);
        writer.write0(data, sheet1,table);
        writer.finish();
        return outputStream.toByteArray();
    }

    /**
     * @Author: jinhaoxun
     * @Description: 使用模型来写入Excel，单sheet，单table
     * @param outputStream Excel的输出流
     * @param data 要写入的以 模型 为单位的数据
     * @param clazz 模型的类
     * @param excelTypeEnum Excel的格式(XLS或XLSX)
     * @Date: 2020/1/16 21:43
     * @Return: byte[]
     * @Throws: Exception
     */
    public static void writeExcel(OutputStream outputStream, List<? extends BaseRowModel> data,
                                  Class<? extends BaseRowModel> clazz, ExcelTypeEnum excelTypeEnum) throws Exception {
        //这里指定需要表头，因为model通常包含表头信息
        ExcelWriter writer = new ExcelWriter(outputStream, excelTypeEnum,true);
        //写第一个sheet, sheet1  数据全是List<String> 无模型映射关系
        Sheet sheet1 = new Sheet(1, 0, clazz);
        writer.write(data, sheet1);
        writer.finish();
    }

    /**
     * @Author: jinhaoxun
     * @Description: 使用模型来写入Excel，单sheet，单table（返回字节数组）
     * @param outputStream Excel的输出流
     * @param data 要写入的以 模型 为单位的数据
     * @param clazz 模型的类
     * @param excelTypeEnum Excel的格式(XLS或XLSX)
     * @Date: 2020/1/16 21:43
     * @Return: byte[]
     * @Throws: Exception
     */
    public static byte[] writeExcel(ByteArrayOutputStream outputStream, List<? extends BaseRowModel> data,
                                    Class<? extends BaseRowModel> clazz, ExcelTypeEnum excelTypeEnum) throws Exception {
        //这里指定需要表头，因为model通常包含表头信息
        ExcelWriter writer = new ExcelWriter(outputStream, excelTypeEnum,true);
        //写第一个sheet, sheet1  数据全是List<String> 无模型映射关系
        Sheet sheet1 = new Sheet(1, 0, clazz);
        writer.write(data, sheet1);
        writer.finish();
        return outputStream.toByteArray();
    }

    /**
     * @Author: jinhaoxun
     * @Description: 使用模型来写入Excel，多sheet，单table （返回字节数组）
     * @param outputStream Excel的输出流
     * @param sheetName  sheet名集合
     * @param datas  要写入的以 模型 为单位的数据
     * @param clazzs  模型的类
     * @param excelTypeEnum  Excel的格式(XLS或XLSX)
     * @Date: 2020/1/16 21:43
     * @Return: byte[]
     * @Throws: Exception
     */
    public static byte[] writeExcel(ByteArrayOutputStream outputStream,List<String> sheetName,List<List<? extends BaseRowModel>> datas,
                                    List<Class<? extends BaseRowModel>> clazzs, ExcelTypeEnum excelTypeEnum) throws Exception {
        //这里指定需要表头，因为model通常包含表头信息
        ExcelWriter writer = new ExcelWriter(outputStream, excelTypeEnum,true);
        if (sheetName.size()!=datas.size()||datas.size()!=clazzs.size()){
            throw new ArrayIndexOutOfBoundsException();
        }
        int i = 0;
        //写第一个sheet, sheet1  数据全是List<String> 无模型映射关系
        for (String name:sheetName){
            Sheet sheet1 = new Sheet(1, 0, clazzs.get(i));
            sheet1.setSheetName(name);
            writer.write(datas.get(i), sheet1);
        }
        writer.finish();
        return outputStream.toByteArray();
    }

    /**
     * @Author: jinhaoxun
     * @Description: 使用模型来写入Excel，多sheet，多table
     * @param outputStream  Excel的输出流
     * @param sheetAndTable sheet和table名，格式：<sheet名，<table名集合>>
     * @param data   <sheet名，<table名，table数据集>>
     * @param clazz  <sheet名，<table名，table数据集实体class类型>>
     * @param excelTypeEnum Excel的格式(XLS或XLSX)
     * @Date: 2020/1/16 21:43
     * @Return: byte[]
     * @Throws: Exception
     */
    public static byte[] writeExcel(ByteArrayOutputStream outputStream,Map<String,List<String>> sheetAndTable,
                                    Map<String,Map<String,List<? extends BaseRowModel>>> data,Map<String,Map<String,Class<? extends BaseRowModel>>> clazz,
                                    ExcelTypeEnum excelTypeEnum) throws Exception {

        //这里指定需要表头，因为model通常包含表头信息
        ExcelWriter writer = new ExcelWriter(outputStream, excelTypeEnum,true);

        Iterator<Map.Entry<String, List<String>>> iterator = sheetAndTable.entrySet().iterator();
        int sheetNo = 1;
        //遍历sheet
        while (iterator.hasNext()){
            Map.Entry<String, List<String>> next = iterator.next();
            //当前sheet名
            String sheetName = next.getKey();
            //当前sheet对应的table的实体类class对象集合
            Map<String, Class<? extends BaseRowModel>> tableClasses = clazz.get(sheetName);
            //当前sheet对应的table的数据集合
            Map<String, List<? extends BaseRowModel>> dataListMaps = data.get(sheetName);
            Sheet sheet = new Sheet(sheetNo, 0);
            sheet.setSheetName(sheetName);
            int tableNo = 1;
            Iterator<Map.Entry<String, Class<? extends BaseRowModel>>> iterator1 = tableClasses.entrySet().iterator();
            //遍历table
            while (iterator1.hasNext()){
                Map.Entry<String, Class<? extends BaseRowModel>> next1 = iterator1.next();
                //当前table名
                String tableName = next1.getKey();
                //当前table对应的class
                Class<? extends BaseRowModel> tableClass = next1.getValue();
                //当前table对应的数据集
                List<? extends BaseRowModel> tableData = dataListMaps.get(tableName);
                Table table = new Table(tableNo);
                table.setClazz(tableClass);
                writer.write(tableData, sheet, table);
                tableNo++;
            }
            sheetNo++;
        }
        writer.finish();
        return outputStream.toByteArray();
    }
}
```

##### ExcelConvertCsvUtil

```
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

/**
 * @Description: excel文件转成csv格式
 * @Author: jinhaoxun
 * @Date: 2020/4/14 上午10:07
 * @Version: 1.0.0
 */
public class ExcelConvertCsvUtil {

    /**
     * @Author: jinhaoxun
     * @Description: 将excel字节码数组转成csv字节码数组
     * @param bytes excel字节码数组
     * @Date: 2020/1/16 21:43
     * @Return: byte[]
     * @Throws: Exception
     */
    public static byte[] convertExcelToCsv(byte[] bytes) throws Exception {
        InputStream inputStream = new ByteArrayInputStream(bytes);
        Workbook wb = WorkbookFactory.create(inputStream);

        String buffer = "";
        Sheet sheet = null;
        Row row = null;
        List<Map<String,String>> list = null;
        String cellData = null;

        if(wb != null){
            //用来存放表中数据
            list = new ArrayList<Map<String,String>>();
            //获取第一个sheet
            sheet = wb.getSheetAt(0);
            //获取最大行数
            int rownum = sheet.getPhysicalNumberOfRows();
            //获取第一行
            row = sheet.getRow(0);
            //获取最大列数
            int colnum = row.getPhysicalNumberOfCells();
            for (int i = 0; i<rownum; i++) {
                row = sheet.getRow(i);
                for (int j = 0; j < colnum; j++) {
                    cellData = (String) getCellFormatValue(row.getCell(j));
                    buffer +=cellData;
                }
                buffer = buffer.substring(0, buffer.lastIndexOf(",")).toString();
                buffer += "\n";
            }

            return buffer.getBytes();
        }
        return null;
    }

    /**
     * @Author: jinhaoxun
     * @Description: 读取excel
     * @param filePath 本地文件路径
     * @Date: 2020/1/16 21:43
     * @Return: Workbook
     * @Throws: Exception
     */
    public static Workbook readExcel(String filePath){
        Workbook wb = null;
        if(filePath==null){
            return null;
        }
        String extString = filePath.substring(filePath.lastIndexOf("."));
        InputStream is = null;
        try {
            is = new FileInputStream(filePath);
            if(".xls".equals(extString)){
                return wb = new HSSFWorkbook(is);
            }else if(".xlsx".equals(extString)){
                return wb = new XSSFWorkbook(is);
            }else{
                return wb = null;
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return wb;
    }

    /**
     * @Author: jinhaoxun
     * @Description:
     * @param cell
     * @Date: 2020/1/16 21:43
     * @Return: Workbook
     * @Throws: Exception
     */
    public static Object getCellFormatValue(Cell cell){
        Object cellValue = null;
        if(cell!=null){
            //判断cell类型
            switch(cell.getCellType()){
                case NUMERIC:{
                    cellValue = String.valueOf(cell.getNumericCellValue()).replaceAll("\n", " ") + ",";
                    break;
                }
                case FORMULA:{
                    //判断cell是否为日期格式
                    if(DateUtil.isCellDateFormatted(cell)){
                        //转换为日期格式YYYY-mm-dd
                        cellValue = String.valueOf(cell.getDateCellValue()).replaceAll("\n", " ") + ",";;
                    }else{
                        //数字
                        cellValue = String.valueOf(cell.getNumericCellValue()).replaceAll("\n", " ") + ",";;
                    }
                    break;
                }
                case STRING:{
                    cellValue = cell.getRichStringCellValue().getString().replaceAll("\n", " ") + ",";;
                    break;
                }
                default:
                    cellValue = "";
            }
        }else{
            cellValue = "";
        }
        return cellValue;
    }
}
```

#### 第五步，编写单元测试类，EasyexcelApplicationTests，并进行测试，使用方法基本都有注释

```
import com.alibaba.excel.metadata.BaseRowModel;
import com.alibaba.excel.support.ExcelTypeEnum;
import com.jinhaoxun.easyexcel.entity.ExcelModel;
import com.jinhaoxun.easyexcel.entity.ExcelModel1;
import com.jinhaoxun.easyexcel.entity.ExcelModel2;
import com.jinhaoxun.easyexcel.util.DataConvertUtil;
import com.jinhaoxun.easyexcel.util.ExcelConvertCsvUtil;
import com.jinhaoxun.easyexcel.util.ExcelUtil;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

import java.io.*;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Slf4j
// 获取启动类，加载配置，确定装载 Spring 程序的装载方法，它回去寻找 主配置启动类（被 @SpringBootApplication 注解的）
@SpringBootTest
class EasyexcelApplicationTests {

    @Test
    void readExcelTest() throws Exception {
        //读取excel
        File file = new File("E:\\2.xlsx");
        InputStream inputStream = new FileInputStream(file);
        //导入excle
        List<ExcelModel> datas = ExcelUtil.readExcel(inputStream, ExcelModel.class, ExcelTypeEnum.XLSX);
        log.info(datas.toString());
    }

    @Test
    void writeExcelTest() throws Exception {
        //单sheet,单table导出测试
        List<ExcelModel> excelModelList = new ArrayList<ExcelModel>();
        for (int i = 0;i<5;i++){
            ExcelModel excelModel = new ExcelModel("日期"+i, "上班时间" + i,
                    "下班时间" + i, "加班时长" + i, "备注" + i);
            excelModelList.add(excelModel);
        }
        File file1 = new File("E:\\2.xlsx");
        ByteArrayOutputStream outputStream1 = new ByteArrayOutputStream();
        byte[] bytes = ExcelUtil.writeExcel(outputStream1, excelModelList, ExcelModel.class, ExcelTypeEnum.XLSX);
        FileOutputStream outputStream = new FileOutputStream(file1);
        outputStream.write(bytes);
    }

    @Test
    void writeExcelTest1() throws Exception {
        //多sheet，多table导出测试，数据集制作
        List<ExcelModel> excelModelList = new ArrayList<ExcelModel>();
        List<ExcelModel1> excelModel1List = new ArrayList<ExcelModel1>();
        List<ExcelModel2> excelModel2List = new ArrayList<ExcelModel2>();
        for (int i = 0;i<5;i++){
            ExcelModel excelModel = new ExcelModel("日期"+i, "上班时间" + i,
                    "下班时间" + i, "加班时长" + i, "备注" + i);
            ExcelModel1 excelModel1 = new ExcelModel1("日期"+i, "上班时间" + i,
                    "下班时间" + i, "加班时长" + i, "备注" + i);
            ExcelModel2 excelModel2 = new ExcelModel2("日期"+i, "上班时间" + i,
                    "下班时间" + i, "加班时长" + i, "备注" + i);

            excelModelList.add(excelModel);
            excelModel1List.add(excelModel1);
            excelModel2List.add(excelModel2);
        }
        Map<String,List<String>> sheetAndTable = new HashMap<String, List<String>>();
        //构造第一个sheet，此sheet内有两个table
        List<String> sheet1tableNames = new ArrayList();
        sheet1tableNames.add("表一");
        sheet1tableNames.add("表二");
        //构造第二个sheet，此sheet内有一个table
        List<String> Sheet2tableNames = new ArrayList<String>();
        Sheet2tableNames.add("表三");
        sheetAndTable.put("sheet1",sheet1tableNames);
        sheetAndTable.put("sheet2",Sheet2tableNames);

        Map<String,Map<String,Class<? extends BaseRowModel>>> clazz = new HashMap<String, Map<String, Class<? extends BaseRowModel>>>();
        //第一个sheet
        Map<String,Class<? extends BaseRowModel>> tables = new HashMap<String, Class<? extends BaseRowModel>>();
        tables.put("表一",ExcelModel.class);
        tables.put("表二",ExcelModel1.class);
        clazz.put("sheet1",tables);
        Map<String,Class<? extends BaseRowModel>> tables1 = new HashMap<String, Class<? extends BaseRowModel>>();
        tables1.put("表三",ExcelModel2.class);
        clazz.put("sheet2",tables1);

        Map<String,Map<String,List<? extends BaseRowModel>>> data= new HashMap<String, Map<String, List<? extends BaseRowModel>>>();
        //第一个sheet
        Map<String,List<? extends BaseRowModel>> map1 = new HashMap<String, List<? extends BaseRowModel>>();
        map1.put("表一",excelModelList);
        map1.put("表二",excelModel1List);
        data.put("sheet1",map1);
        Map<String,List<? extends BaseRowModel>> map2 = new HashMap<String, List<? extends BaseRowModel>>();
        map2.put("表三",excelModel2List);
        data.put("sheet2",map2);

        File file1 = new File("E:\\3.xlsx");
        ByteArrayOutputStream outputStream1 = new ByteArrayOutputStream();
        byte[] bytes = ExcelUtil.writeExcel(outputStream1,sheetAndTable,data,clazz,ExcelTypeEnum.XLSX);
        FileOutputStream outputStream = new FileOutputStream(file1);
        outputStream.write(bytes);
    }

    @Test
    void convertExcelToCsvTest() throws Exception {
        //读取excel
        File file = new File("/Users/ao/Desktop/activitycode_20200414102350.xlsx");
        InputStream inputStream = new FileInputStream(file);
        byte[] bytes = DataConvertUtil.inputStreamTobyte2(inputStream);
        ExcelConvertCsvUtil.convertExcelToCsv(bytes);
    }

    @BeforeEach
    void testBefore(){
        log.info("测试开始!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!");
    }

    @AfterEach
    void testAfter(){
        log.info("测试结束!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!");
    }

}
```

##### 完整代码地址：https://github.com/Jinhx128/springboot-demo

##### 注：此工程包含多个module，本文所用代码均在easyexcel-demo模块下