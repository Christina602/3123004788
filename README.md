# 3123004788
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileWriter;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.Scanner;

public class PlagiarismChecker {

    // 计算相似度的方法
    public static double calculateSimilarity(String text1, String text2) {
        // 使用简单的字符匹配算法（可以替换为更复杂的算法，如余弦相似度等）
        int matches = 0;
        int totalLength = Math.max(text1.length(), text2.length());

        for (int i = 0; i < Math.min(text1.length(), text2.length()); i++) {
            if (text1.charAt(i) == text2.charAt(i)) {
                matches++;
            }
            }

        return (double) matches / totalLength;
    }

    // 主函数
    public static void main(String[] args) {
        if (args.length < 3) {
            System.out.println("请提供原文文件路径、抄袭版文件路径和输出文件路径。");
            return;
        }

        String originalFilePath = args[0];
        String plagiarizedFilePath = args[1];
        String outputFilePath = args[2];

        try {
            // 读取原文文件内容
            String originalText = new String(Files.readAllBytes(Paths.get(originalFilePath)), StandardCharsets.UTF_8);

            // 读取抄袭版文件内容
            String plagiarizedText = new String(Files.readAllBytes(Paths.get(plagiarizedFilePath)), StandardCharsets.UTF_8);

            // 计算相似度
            double similarity = calculateSimilarity(originalText, plagiarizedText);

            // 将结果写入输出文件，精确到小数点后两位
            try (FileWriter writer = new FileWriter(outputFilePath)) {
             writer.write(String.format("%.2f", similarity));
            }

            System.out.println("重复率计算完成，结果已保存到 " + outputFilePath);

        } catch (FileNotFoundException e) {
            System.out.println("文件未找到，请检查文件路径是否正确。");
        } catch (IOException e) {
            System.out.println("发生错误: " + e.getMessage());
        }
    }
}
