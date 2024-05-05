% This must be in the first 5 lines to tell arXiv to use pdfLaTeX, which is strongly recommended.
\pdfoutput=1
% In particular, the hyperref package requires pdfLaTeX in order to break URLs across lines.

\documentclass[11pt]{article}

% Change "review" to "final" to generate the final (sometimes called camera-ready) version.
% Change to "preprint" to generate a non-anonymous version with page numbers.
\usepackage[review]{acl}

% Standard package includes
\usepackage{times}
\usepackage{latexsym}

% For proper rendering and hyphenation of words containing Latin characters (including in bib files)
\usepackage[T1]{fontenc}
% For Vietnamese characters
% \usepackage[T5]{fontenc}
% See https://www.latex-project.org/help/documentation/encguide.pdf for other character sets

% This assumes your files are encoded as UTF8
\usepackage[utf8]{inputenc}

% This is not strictly necessary, and may be commented out,
% but it will improve the layout of the manuscript,
% and will typically save some space.
\usepackage{microtype}

% This is also not strictly necessary, and may be commented out.
% However, it will improve the aesthetics of text in
% the typewriter font.
\usepackage{inconsolata}

%Including images in your LaTeX document requires adding
%additional package(s)
\usepackage{graphicx}

% If the title and author information does not fit in the area allocated, uncomment the following
%
%\setlength\titlebox{<dim>}
%
% and set <dim> to something 5cm or larger.

\title{Amazon Product Review Sentiment Analysis}

% Author information can be set in various styles:
% For several authors from the same institution:
% \author{Author 1 \and ... \and Author n \\
%         Address line \\ ... \\ Address line}
% if the names do not fit well on one line use
%         Author 1 \\ {\bf Author 2} \\ ... \\ {\bf Author n} \\
% For authors from different institutions:
% \author{Author 1 \\ Address line \\  ... \\ Address line
%         \And  ... \And
%         Author n \\ Address line \\ ... \\ Address line}
% To start a separate ``row'' of authors use \AND, as in
% \author{Author 1 \\ Address line \\  ... \\ Address line
%         \AND
%         Author 2 \\ Address line \\ ... \\ Address line \And
%         Author 3 \\ Address line \\ ... \\ Address line}

\author{Yermakhan Serikbayev \\
  Affiliation / Address line 1 \\
  Affiliation / Address line 2 \\
  Affiliation / Address line 3 \\
  \texttt{yermakhan.serikbayev@nu.edu.kz} 

%\author{
%  \textbf{First Author\textsuperscript{1}},
%  \textbf{Second Author\textsuperscript{1,2}},
%  \textbf{Third T. Author\textsuperscript{1}},
%  \textbf{Fourth Author\textsuperscript{1}},
%\\
%  \textbf{Fifth Author\textsuperscript{1,2}},
%  \textbf{Sixth Author\textsuperscript{1}},
%  \textbf{Seventh Author\textsuperscript{1}},
%  \textbf{Eighth Author \textsuperscript{1,2,3,4}},
%\\
%  \textbf{Ninth Author\textsuperscript{1}},
%  \textbf{Tenth Author\textsuperscript{1}},
%  \textbf{Eleventh E. Author\textsuperscript{1,2,3,4,5}},
%  \textbf{Twelfth Author\textsuperscript{1}},
%\\
%  \textbf{Thirteenth Author\textsuperscript{3}},
%  \textbf{Fourteenth F. Author\textsuperscript{2,4}},
%  \textbf{Fifteenth Author\textsuperscript{1}},
%  \textbf{Sixteenth Author\textsuperscript{1}},
%\\
%  \textbf{Seventeenth S. Author\textsuperscript{4,5}},
%  \textbf{Eighteenth Author\textsuperscript{3,4}},
%  \textbf{Nineteenth N. Author\textsuperscript{2,5}},
%  \textbf{Twentieth Author\textsuperscript{1}}
%\\
%\\
%  \textsuperscript{1}Affiliation 1,
%  \textsuperscript{2}Affiliation 2,
%  \textsuperscript{3}Affiliation 3,
%  \textsuperscript{4}Affiliation 4,
%  \textsuperscript{5}Affiliation 5
%\\
%  \small{
%    \textbf{Correspondence:} \href{mailto:email@domain}{email@domain}
%  }
%}

\begin{document}
\maketitle



\section{Introduction}

The project fine tunes the BERT model in order to classify Amazon product reviews into positive, neutral, or negative sentiment categories based on the content of the reviews

\section{Data}

The dataset used for the project is "Amazon-Reviews-2023" of McAuley-Lab. The dataset consists of Amazon product reviews collected from the McAuley Lab dataset for the year 2023. The dataset is roughly consists of 700 000 samples. The dataset is consists of columns like: User, Item, ReviweText, Category, Rating, Rtoken, Mtoken. I selected out only "ReviewText" and "Rating". The dataset rating column consists of 5 classes each signifying the grades of: 1, 2, 3, 4, 5. I selected 10000 samples of each class. Finally I classified the grades of 1 and 2 as "negative", 3 as a "neutral", 4 and 5 as a "positive". They will be indexed as 1, 2, 3 correspondingly. 

\section{Methods}
As for the methods I fine-tuned a Distilled BERT model for sentiment analysis on Amazon reviews. First of all I initialized the "bert-base-uncased" model. Afterwards I splitted the prepared dataset into three sets as for training, validation, and testing. Training set contains 21000 samples validation set 3000 and testing set contains 6000 samples. And both of the training batch size and testing batch sizes are 32. The logic for fine tuning is referenced from GitHub repository "bert-product-rating-predictor" by "csbanon". 


\section{Experiments}
Class weights for the weighted cross-entropy loss are computed to handle class imbalance in the dataset. The frequency distribution of labels is analyzed to determine appropriate weights, ensuring fair representation of all classes during training. The results are follwing:
\includegraphics[width=\columnwidth]{image.png}

Then the training loop iterates over epochs, batches, and data samples to train the model. During each epoch, training loss, accuracy, and other metrics are monitored and logged. So the the result of the traing on each epoch and final result is as follows:
\includegraphics[width=\columnwidth]{image1.png}
So the result of the accuracy testing on the testing set shows the validation accuracy of 75.83.
\includegraphics[width=\columnwidth]{image3.png}

\section{Results & analysis}
The training accuracy steadily increases over the course of Epoch 1, starting at 34.375\% for the first batch and reaching 71.88\% by the end of training. So the overall training accuracy for Epoch 1 is 72.19\%. Regarding the training loss it decreases after each iteration reaching 0.6317. Regarding the validation accuracy it reaches 75.74\% after epoch 1 and 75.83\%  after training for 2 epochs.
    The training process demonstrates a consistent improvement in training accuracy over the course of Epoch 1, indicating that the model effectively learns from the training data as it progresses. This upward trend suggests that the model is successfully capturing the underlying patterns in the text reviews. Moreover, the validation accuracy remains consistently high, suggesting that the model generalizes well to unseen data and does not exhibit signs of overfitting to the training set. 


\section*{Conclusion}
In conclusion, The experiments with a fine-tuned Distilled BERT model showed promising results. The training accuracy improved steadily, reaching 72.19\% by the end of the first training round, and the training loss consistently decreased. Validation accuracy also got better, reaching 75.83\% after two rounds of training. These results indicate that following model learned well from the training data and could understand the patterns in the reviews. The high validation accuracy suggests that model can work well with new data without getting too specific to the training set. Overall, following approach seems promising for accurately understanding the sentiment in Amazon product reviews, which could help businesses understand customer feedback better.
\bibliography{custom}
Dataset: \href{https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023}{McAuleyLab/Amazon-Reviews} \newline Model:  \href{https://huggingface.co/docs/transformers/model_doc/bert}{BERT} \newline Codes used: \href{https://github.com/csbanon/bert-product-rating-predictor/blob/master/notebooks/classification/bert-training.ipynb}{csbanon/bert-product-rating-predictor} 

\end{document}
