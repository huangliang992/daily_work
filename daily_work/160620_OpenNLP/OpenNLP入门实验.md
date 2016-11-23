## OpenNLP
也是自然语言分析处理的工具是apache的开源项目，感觉这玩意儿更容易入手，使用的人更多。

### 句子探测的例子

注意，首先，添加opennlp的pom依赖

	<!-- OpenNLP -->
		<dependency>
			<groupId>org.apache.opennlp</groupId>
			<artifactId>opennlp-tools</artifactId>
			<version>1.6.0</version>
		</dependency>

接着还得下载训练集

地址是http://opennlp.sourceforge.net/models-1.5/，例如句子探测，分词都有探测模型使用的是en-sent.bin。

![](http://i.imgur.com/fZ7Y2Mr.png)

在使用的时候得添加训练模型的文件路径

	String path="E:/EclipseWorkSpace/hibernettest/src/main/resources/"
				+ "com/hainan/cs/OpenNLPModels/en-sent.bin";

		// always start with a model, a model is learned from training data
		InputStream is = new FileInputStream(path);
		SentenceModel model = new SentenceModel(is);
		SentenceDetectorME sdetector = new SentenceDetectorME(model);


句子探测代码：

	package com.hainan.cs.opennlp;

	import java.io.FileInputStream;
	import java.io.IOException;
	import java.io.InputStream;

	import opennlp.tools.sentdetect.SentenceDetectorME;
	import opennlp.tools.sentdetect.SentenceModel;

	public class SentenceDetector {
	public static void main(String args[]) throws IOException {
		String paragraph = "Hi. How are you? This is Mike.";
		String path="E:/EclipseWorkSpace/hibernettest/src/main/resources/"
				+ "com/hainan/cs/OpenNLPModels/en-sent.bin";

		// always start with a model, a model is learned from training data
		InputStream is = new FileInputStream(path);
		SentenceModel model = new SentenceModel(is);
		SentenceDetectorME sdetector = new SentenceDetectorME(model);

		String sentences[] = sdetector.sentDetect(paragraph);

		System.out.println(sentences[0]);
		System.out.println(sentences[1]);
		is.close();
	}
	}
句子探测结果：

![](http://i.imgur.com/NAHlyZ3.png)


### 分词的例子
直接上代码，使用的模型在代码里

	package com.hainan.cs.opennlp;

	import java.io.FileInputStream;
	import java.io.IOException;
	import java.io.InputStream;

	import opennlp.tools.tokenize.TokenizerME;
	import opennlp.tools.tokenize.TokenizerModel;
	import opennlp.tools.util.InvalidFormatException;

	public class Tokenizer {
		public static void main(String args[]) throws InvalidFormatException, IOException {
			String paragraph = "Hi. How are you? This is Mike.";
			String path = "E:/EclipseWorkSpace/hibernettest/src/main/resources/"
				+ "com/hainan/cs/OpenNLPModels/en-token.bin";
			InputStream is = new FileInputStream(path);

			TokenizerModel model = new TokenizerModel(is);

			TokenizerME tokenizer = new TokenizerME(model);

			String tokens[] = tokenizer.tokenize("Hi. How are you? This is Mike.");

			for (String a : tokens)
			System.out.println(a);
	
			is.close();
		}
	}

结果：

![](http://i.imgur.com/BuwNg2E.png)

### 名字查找，比如说要识别哪些是人命

	package com.hainan.cs.opennlp;

	import java.io.FileInputStream;
	import java.io.IOException;
	import java.io.InputStream;

	import opennlp.tools.namefind.NameFinderME;
	import opennlp.tools.namefind.TokenNameFinderModel;
	import opennlp.tools.util.Span;

	public class NameFinder {
		public static void main(String args[]) throws IOException {

			String paragraph = "Hi. How are you? This is Mike.";
			String path = "E:/EclipseWorkSpace/hibernettest/src/main/resources/"
				+ "com/hainan/cs/OpenNLPModels/en-ner-person.bin";
			InputStream is = new FileInputStream(path);

			TokenNameFinderModel model = new TokenNameFinderModel(is);
			is.close();

			NameFinderME nameFinder = new NameFinderME(model);

			String[] sentence = new String[] { "Mike", "Smith", "is", "a", "good", "person" };

			Span nameSpans[] = nameFinder.find(sentence);

			for (Span s : nameSpans)
			System.out.println(s.toString());
		}
	}

![](http://i.imgur.com/QHEACuz.png)


### 词性标注

	package com.hainan.cs.opennlp;

	import java.io.File;
	import java.io.IOException;
	import java.io.StringReader;

	import opennlp.tools.cmdline.PerformanceMonitor;
	import opennlp.tools.cmdline.postag.POSModelLoader;
	import opennlp.tools.postag.POSModel;
	import opennlp.tools.postag.POSSample;
	import opennlp.tools.postag.POSTaggerME;
	import opennlp.tools.tokenize.WhitespaceTokenizer;
	import opennlp.tools.util.ObjectStream;
	import opennlp.tools.util.PlainTextByLineStream;

	public class POS {
		public static void main(String args[]) throws IOException {

			String paragraph = "Hi. How are you? This is Mike.";
			String path = "E:/EclipseWorkSpace/hibernettest/src/main/resources/"
				+ "com/hainan/cs/OpenNLPModels/en-pos-maxent.bin";
			POSModel model = new POSModelLoader().load(new File(path));
			PerformanceMonitor perfMon = new PerformanceMonitor(System.err, "sent");
			POSTaggerME tagger = new POSTaggerME(model);

			String input = "Hi. How are you? This is Mike.";
			ObjectStream<String> lineStream = new PlainTextByLineStream(new StringReader(input));

			perfMon.start();
			String line;
			while ((line = lineStream.read()) != null) {

				String whitespaceTokenizerLine[] = WhitespaceTokenizer.INSTANCE.tokenize(line);
				String[] tags = tagger.tag(whitespaceTokenizerLine);

				POSSample sample = new POSSample(whitespaceTokenizerLine, tags);
				System.out.println(sample.toString());
	
				perfMon.incrementCounter();
			}
			perfMon.stopAndPrintFinalResult();
		}
	}

结果：

![](http://i.imgur.com/KdZVpBj.png)


### Chunker

	package com.hainan.cs.opennlp;

	import java.io.File;
	import java.io.FileInputStream;
	import java.io.IOException;
	import java.io.InputStream;
	import java.io.StringReader;

	import opennlp.tools.chunker.ChunkerME;
	import opennlp.tools.chunker.ChunkerModel;
	import opennlp.tools.cmdline.PerformanceMonitor;
	import opennlp.tools.cmdline.postag.POSModelLoader;
	import opennlp.tools.postag.POSModel;
	import opennlp.tools.postag.POSSample;
	import opennlp.tools.postag.POSTaggerME;
	import opennlp.tools.tokenize.WhitespaceTokenizer;
	import opennlp.tools.util.ObjectStream;
	import opennlp.tools.util.PlainTextByLineStream;
	import opennlp.tools.util.Span;

	public class Chunker {
		public static void main(String args[]) throws IOException {

			String path = "E:/EclipseWorkSpace/hibernettest/src/main/resources/"
				+ "com/hainan/cs/OpenNLPModels/en-pos-maxent.bin";
			POSModel model = new POSModelLoader().load(new File(path));
			PerformanceMonitor perfMon = new PerformanceMonitor(System.err, "sent");
			POSTaggerME tagger = new POSTaggerME(model);

			String input = "Hi. How are you? This is Mike.";
			ObjectStream<String> lineStream = new PlainTextByLineStream(new StringReader(input));

			perfMon.start();
			String line;
			String whitespaceTokenizerLine[] = null;

			String[] tags = null;
			while ((line = lineStream.read()) != null) {
				whitespaceTokenizerLine = WhitespaceTokenizer.INSTANCE.tokenize(line);
				tags = tagger.tag(whitespaceTokenizerLine);

				POSSample sample = new POSSample(whitespaceTokenizerLine, tags);
				System.out.println(sample.toString());
				perfMon.incrementCounter();
			}
			perfMon.stopAndPrintFinalResult();

			// chunker
			String path1 = "E:/EclipseWorkSpace/hibernettest/src/main/resources/"
				+ "com/hainan/cs/OpenNLPModels/en-chunker.bin";

			InputStream is = new FileInputStream(path1);
			ChunkerModel cModel = new ChunkerModel(is);

			ChunkerME chunkerME = new ChunkerME(cModel);
			String result[] = chunkerME.chunk(whitespaceTokenizerLine, tags);

			for (String s : result)
				System.out.println(s);

			Span[] span = chunkerME.chunkAsSpans(whitespaceTokenizerLine, tags);
			for (Span s : span)
				System.out.println(s.toString());
		}
	}

结果： 看不懂这是个啥求，先放着吧

![](http://i.imgur.com/gkgJnaC.png)


###  分析器parser

	package com.hainan.cs.opennlp;

	import java.io.FileInputStream;
	import java.io.IOException;
	import java.io.InputStream;

	import opennlp.tools.cmdline.parser.ParserTool;
	import opennlp.tools.parser.Parse;
	import opennlp.tools.parser.ParserFactory;
	import opennlp.tools.parser.ParserModel;
	import opennlp.tools.util.InvalidFormatException;

	public class Parser {

		public static void main(String args[]) throws InvalidFormatException, 	IOException {
			// http://sourceforge.net/apps/mediawiki/opennlp/index.php?title=Parser#Training_Tool

			String path = "E:/EclipseWorkSpace/hibernettest/src/main/resources/"
				+ "com/hainan/cs/OpenNLPModels/en-parser-chunking.bin";
			InputStream is = new FileInputStream(path);

			ParserModel model = new ParserModel(is);

			Parser parser = ParserFactory.create(model);

			String sentence = "Programcreek is a very huge and useful website.";
			Parse topParses[] = ParserTool.parseLine(sentence, (opennlp.tools.parser.Parser) parser, 1);

			for (Parse p : topParses)
				p.show();

			is.close();

			/*
			 * (TOP (S (NP (NN Programcreek) ) (VP (VBZ is) (NP (DT a) (ADJP (RB
			 * very) (JJ huge) (CC and) (JJ useful) ) ) ) (. website.) ) )
			 */
		}
	}

结果： 
 
![](http://i.imgur.com/IH5hXOt.png)

自然语言处理两个最重要的方面就是词性标注和句法分析。例子都有了。
