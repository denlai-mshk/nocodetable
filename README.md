# No-Code Hacks for Tackling Non-Standard Table Recognition in GenAI
## Challenge of data extraction accuracy from non-standard tables
The year 2024 marks the second year of the new era of Gen AI. Many AI engineers recognize that the biggest challenge in RAG (Retrieval-Augmented Generation) search patterns is maintaining high accuracy in generated answers. This topic is very popular in the AI community because the quality of pre-processing in the document ingestion pipeline is crucial for accuracy. One major challenge is table recognition, specifically how to extract data relationships across multiple columns and sub-columns. For example, financial documents and global PSG reports often have non-standard table structures. These complexities make it essential to focus on improving pre-processing techniques to ensure high accuracy in generated answers.

## No one shoe fits all
Intuitively, we might think that the GPT model can read the document directly. However, this is not the case. For extracting text-based data from documents, we need an OCR service like Azure Document Intelligence to help us read documents like PDFs or DOCs and transform the data into JSON format for pre-processing. However, we all know that there are over a billion document styles in the world. The challenge for OCR to recognize complicated tables and artistic layouts is an ongoing battle that cannot be resolved by one approach or a single model. Therefore, we must focus on pre-processing to refine the OCR results adaptively, especially for data represented in non-standard table layouts.

## Non-Standard 2D Table Structures
Since 2022, I've been working on pre-processing document extraction pipelines, focusing on non-standard tables. Here are common patterns:
- Combined columns in one row
- Combined rows in one column
- Mixture of the above
- Headline context in the first and last row
- Bilingual context in one cell
  
You might wonder, if a cell can be located by (x, y), doesn't that make it a standard 2D table? Actually, not quite. Standard 2D tables have a fixed number of rows and columns, whereas non-standard tables have a dynamic number of rows and columns. For example, a 3x4 table with 12 cells is standard. If you observe more or fewer cells, it becomes non-standard.
 
## No more coding in the GenAI era
In the past, many approaches to tackling non-standard table extraction were based on programming. These methods worked, but you had to constantly maintain the codebase to support new non-standard table layouts that your existing code couldn't recognize. Now, by leveraging the power of GPT-4o 2024-05-13 and the latest 2024-02-29 preview model from Azure Document Intelligence, your non-standard table recognition mechanism can evolve to the next level, a **NO-CODE** approach.

## De-normalizing Tables by Prompt Engineering
Although LLM models are now capable of reading data written in markdown and JSON formats and understanding data correlation relationships, this is not the best approach since the data relationships are still stored implicitly. For example, if a child asks their mother what time it is, the mother can give two answers: the first one is "7 o'clock," and the second one is "(24-3)/3." Which answer is the most straightforward and least confusing? Definitely, "7 o'clock" because it is directly in language, with no math involved. Therefore, if we can convert data relationships into clear English statements, LLM models can handle them with minimal effort, perform faster, and deliver the highest accuracy. The diagram below illustrates this refactorization:


Although we mentioned no code is required, it doesn't mean this can be achieved through configuration alone. We still need to write a smart prompt to instruct GPT-4o to perform the tasks that coding would typically handle. Using the prompt template below, GPT-4o can read content in markdown format and flatten data into row-based statements. The trick we use here is to ask GPT-4o to automatically refill any missing cell values based on its understanding of the markdown of non-standard 2D table structures while de-normalizing all the rows against their columns. Furthermore, the prompt instructs GPT-4o to mark any generated cell value with a watermark "{{auto-fill}}". This watermark allows us to recognize that this cell value is artificially generated, notifying us to double-check this value in our evaluation pipeline.
 

After GPT-4o flattens the data, we obtain a set of single statements that encompass all related information, including the table name and row item context in relation to its columns. This row-based format helps LLM models understand the semantic relationships between all data segments (row items against all columns). Additionally, consolidating all related data into one statement enhances the confidence score in AI search results, as the indexing algorithm is based on similarity.

## Sample A: Combined row item across columns and sub-column


## Sample B: Combined column value for multiple rows


## Closing
In summary, recognizing non-standard tables is a big challenge in AI, but with the right tools and techniques, we can make significant progress. 


