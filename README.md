# Hypernym_inferring

Hypernym discovery problem aims to extract and discover such noun pairs that one noun is a hypernym of the other.

xx, xx, xx and xx. Supervised Distributional Hypernym Discovery via Domain Adaptation. Proceedings of the 2016 Conference on Empirical Methods in Natural Language Processing (EMNLP), pages 424–435, Austin, Texas, USA, 1-5 November 2016.


##
Training disambiguated hyponym-hypernym pairs from Wikidata and KB-Unify (Delli Bovi et al., 2015), as well as domain-filtered SensEmbed (Iacobacci et al., 2015) vectors and NASARI domain labels (Camacho-Collados et al., 2017) can be [downloaded](https://github.com/Jack-Cherish/Machine-Learning/blob/master/Regression/regression_old.py) from here.


## Train
The first step trains a linear transformation matrix inspired by Mikolov et al. (2013) and Fu et al. (2014). The matrix is trained following the code from here.

`python train_transMat.py <training_data> <embeddings> <domains_file> <input_domain> <expand>                                          ` 
* <training_data> is a two-column text file, where column 1 has terms, and column 2 has hypernyms.
* <embeddings> is a binary or text file with embeddings in word2vec format (first line of the file should contain the following info: 
* <num_vecs><space><numb_dimensions>.
* <domains_file> tab-separated file, column 1 has synsets, column 2 has their associated domain (e.g. bn:david_bowie<tab>Music)
* <input_domain> is a knowledge domain, e.g. 'music' or 'media'. It can have value ' ' for skipping the domain-wise term filtering.
* <expand> is an optional argument which can have either 'expanded' or ' ' value. Use "expand" if your data is at synset level, and your embeddings are at sense level. For example, a training instance may contain the synset performance.n.01 and you may have embeddings like performance_wn:performance and performance_wn:public_presentation.

The above saves a filtered vectors file to speed up testing, called vectors_domain.txt, and the transformation matrix in text format transformation_matrix+_domain+.txt, or transformation_matrix_general.txt if <input_domain>='none'.


## Predict
Apply the learned transformation matrix to your test data with:

 `python batch_predict.py <data> <matrix_file> <embeddings> <topn> <expanded>                                                          ` 
* <data> text file, with one concept per line.
* <matrix_file> obtained in step 1.
* <embeddings> the same ones used in step 1, or the filtered ones.
* <topn> Top n candidate hypernyms to be retrieved for each input term.
* <expanded> same flag as in step 1.

You can also play interactively in the python console, with:

`python interactive_predict.py <matrix_file> <embeddings> <topn> <expanded>                                                         ` 

The program will prompt you for a concept name, until you type 'leave'.


## Evaluate
Evaluate the performance of your hypernym discovery algorithm with:

`python evaluate.py <gold_file> <predictions_file>                                                                                     ` 
* <gold_file> Text file where column 1 is the term, and from column 2 are valid hypernyms.
* <predictions_file> Text file where column 1 is the term, and columns 2 onwards are either <candidate><space><score> pairs, or simplpy the candidates.

It will save a <predictions_file>+__results.txt file with classic IR metrics computed on your data, such as MRR, MAP, P@K and R-P. The code for computing these metrics comes from here.


## References
Camacho-Collados, J., & Navigli, R. (2017). BabelDomains: Large-Scale Domain Labeling of Lexical Resources. EACL 2017, 223.

Iacobacci, I., Pilehvar, M. T., & Navigli, R. (2015). SensEmbed: Learning Sense Embeddings for Word and Relational Similarity. In ACL (1) (pp. 95-105).

Mikolov, T., Le, Q. V., & Sutskever, I. (2013). Exploiting similarities among languages for machine translation. arXiv preprint arXiv:1309.4168.

Fu, R., Guo, J., Qin, B., Che, W., Wang, H., & Liu, T. (2014). Learning Semantic Hierarchies via Word Embeddings. In ACL (1) (pp. 1199-1209).







