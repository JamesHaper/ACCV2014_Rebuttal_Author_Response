ACCV2014_Rebuttal_Author_Response
=================================

ACCV2014_Rebuttal_Author_Response

--------------------------------------------------------------------------------------------------
Assigned_Reviewer_3
==================================================================================================
Weakness (Cons)

1.	From my understanding, at the end of Sec 3.2, each character is represented as BoW of CHoG features extracted from each pixel. How then would this representation be comparable to a different image (or character) that has a different “visual vocabulary”. How can an SVM be now learnt on such a representation?

Thanks for pointing this out. The final descriptor of a image (or character) is a histogram of the visual vocabulary, which is assigned by finding the nearest global vocabulary in the visual code book. We failed to realize that people who are not familiar with BoW model might be confused. We will add this detail in revision.

==================================================================================================
2.	It is not clear how clusters can be “added” after a few iterations of the K-means algorithm. Wouldn’t the K-means need to restart if the K changes?

As suggested by the reviewer, we did restart after the K changed. The computational cost of performing multiple K-means clustering is discussed in Sec 4. We apologize for the misuse of the word "add", which is confusing.

==================================================================================================
3.	The feature apparently concatenates the distance between the centroid and the feature. How is the concatenated distance used when learning a classifier such as an SVM?

We apologize again for our writing, which prevented us from expressing ourself clearly. In Sec 3.2 we said that "Finally, to exploit the spatial characteristic we concatenate the normalized distance between the centroid of the compressed features to the compressed feature vectors", we intended to say "... the normalized distance between the centroid of the compressed features and the center of the image (or character) to the compressed feature vectors". We define the distance as the centroids of the clusters to the center of the image, and it is directly concatenated to the feature vector to build the “global” vocabulary before training.

Through experiments we observe that by concatenating the distance to the feature we can generate a vocabulary that is slightly more discriminative, which helped us to improve the performance. This is because in a rotation invariant setting it is really difficult to tell the difference between some subsets such as "0","o","O", and "U","C", because the CHOG features of the characters in these subsets are really similar to one another. By introducing the aforementioned distance we can both keep the rotation invariance and build a more discriminative vocabulary on these special characters, since in general "0" are more slender than "O","o", and "U" more slender than "C". After concatenating the distance, different vocabulary can be easily generated for different categories of characters, thus improving the performance.

==================================================================================================
4.	What are the small ones, medium ones and larger ones at Ln 333-336? 

As described in Sec 3.1, we used a multi-scale representation. By using window functions of different sizes we extracted feature vectors of different lengths. The adjectives refer to the size of the window functions. The length of the feature vectors is related to the radius of the surrounding area we are trying to describe. It makes sense to use larger vectors for larger area because it contains more information. The combination of the multi-scale representations helped to improve the performance. We will include the experimental results if there is still enough space after other revisions.

==================================================================================================
Other comments

1.	The writing could be greatly improved, and the solution better explained, given that there are two more pages available for the content. 

We will revise our paper accordingly. We did more experiments on various settings such as using one-vs-all SVM as the final classifier, and using dense SIFT and dense SUFR as the rotation invariant feature after the initial submission, and we will include them if possible.

==================================================================================================
2.	The reference to the CHoG paper by Chandrasekhar et al. is missing. Either the CVPR or the IJCV version should have been cited, given that the approach is based entirely on it.

We will add appropriate reference. It was removed in the editing process by mistake.

==================================================================================================
3.	Details of the Multi-Class SVM are missing. What Kernel was used here? Typically multi-class SVMs perform poorer than a combination of 1-vs-all SVMs. Is there any particular reason to choose multi-class SVMs here?

We used RBF kernel. Although multi-class SVMs perform poorer, the probability estimation of its prediction is well defined. The probability output could potentially help word recognition. We intend to build our own end-to-end scene text recognition in future work, which includes probabilistic language model for word recognition, by the time the paper was submitted we only included experimental results on Multi-Class SVM. Just as the reviewer said, thorough evaluations of various settings could have been performed as part of this work. We did more experiments after our initial submission, and we would like to include these new information in our revised paper.

==================================================================================================
4.	There is a sentence repeated at Ln 113-115. Is the SIFT vector “fixed” at 128 by the authors of [8] (Ln. 196)? The explanation of a linear SVM is not quite necessary, the community understands this tool well enough. Ln. 416 is not needed. 

Sorry for failing to proofreading our paper carefully enough. The SFIT vector is not intentionally fixed by the author of [8], from our understanding, they just used standard implementation, which differs from our approach where we used feature vector of different length to describe the image with different level of details. 

==================================================================================================
5.	Where did the number 250 come from at Ln. 355? Is that the size of the vocabulary learnt from compressed CHoG features? Is [8] really extracting features at every other pixel, as alluded to by the 24x24 at Ln. 356?

Yes, it is the size of the vocabulary learnt from compressed CHoG features. This number comes from Fig 4. With it we achieved the best performance on the SVT-Perspective dataset. 

As for the feature extracting in [8], after further examination we realized that it should be 12*12, we misunderstood the meaning of the "grid" in [8]. The correct computational workload is 13.04% of the method in [8], which checks out with the actual processing time consumed in reality (3.5 seconds compared to 38.6 seconds, time consumed in feature extraction included). Thanks for pointing this out, we were confused by the discrepancies in theoretical analysis and actual experiments.

==================================================================================================
6.	Several typos/grammatical errors: perceptive, every pixels, persecutive, method have been, wild variety, etc. Use forward quotes (``) at Ln 68-69.

We will correct these errors.

==================================================================================================
7.	Shouldn’t Ln 425 be comparing with [24] instead of the poorer performing [22]?

Yes, it was a mistake.

==================================================================================================
8.	Provide a citation for the “state of the art” method at Ln. 530. 

We will add the citation.

==================================================================================================
9.	In the references, conference papers should be cited as “In Proc.:”. [23] could have pointed to Abbyy’s URL instead of the magazine article. 

We will change it accordingly.



Assigned_Reviewer_7
==================================================================================================
Weakness (Cons)
1.	First, I suspect the extracted feature in (1) is really rotation-invaraint or not. Eq. (1) shows that the feature depends on the given direction n, and thus, is sensitive to rotation.

Thanks for pointing this out. We made a mistake in the editing process. CHOG is actually presented in terms of the orthogonal (periodic) circular Fourier basis functions, which was not included in this work. The Fourier basis is used for a continuous and rotation friendly representation.

In this work we only intended to present the form of this feature but not how CHOG works, and we were going to refer the reader to the CHOG paper about its representation in Fourier domain. However it was removed in the editing process by mistake, together with its corresponding reference (As suggested by another reviewer). We will correct this in our revised paper.

==================================================================================================
2.	I suspect the necessity of dense feature compression by k-means clustering. Does this mean the clustering of features for each image? If so, it causes run-time clustering cost on the image. Because the feature vector for classification equals the size of codebook, the clustering of dense features actually does not change the dimensionality. So, is the dense feature compression benefitial to classification?

We explained that for character recognition in natural scene images dense feature is important, as already mentioned by previous works. This is because keypoint detection methods fail to produce enough high quality keypoints for meaningful classification in this task. Sure we introduced more complexity by clustering, but we are able to compress the densely extracted features to an essential subset, in which all the features are distinctive from one another. Through this we can benefit from the discriminative power of densely extracted features and we can build the final descriptor of the characters with higher efficiency, because it is a histogram that is assigned by looking for the nearest visual words for every compressed features instead of the original densely extracted features.

In Sec 4, we discussed the computational cost and compared our method to the state-of-the-art perspective character recognition work. Clearly the cost of clustering is not trivial, but the compressed features is significantly simpler. The combined computational workload of clustering and building the final descriptor is still significantly smaller than directly using densely extracted features.

==================================================================================================
3.	Eq. (2), this is not a SVM, but a least square regression model.

Indeed, the optimization problem of the linear SVM is very similar to that of the least square regression model. But these two are not exactly the same. We will further elaborate on the difference.

==================================================================================================
4.	Implementation details: why the CHOG feature dimensionality is 60? "average" means the dimensionality is variable? Why and how?

As described in Sec 3.1, we used a multi-scale representation. By using window functions of different sizes we extracted feature vectors of different lengths. The length of the feature vectors is related to the radius of the surrounding area we are trying to describe. It makes sense to use larger vectors for larger area because it contains more information. Since the length of the feature vectors of different scales are different, in order to compare the theoretical computational cost of our method to the cost of [8], we used the average length of the feature vectors.

==================================================================================================
5.	Line 354, what is 250?

It is the size of the vocabulary learnt from compressed CHoG features. This number comes from Fig 4. With it we achieved the best performance on the SVT-Perspective dataset. 

==================================================================================================
6.	Refs 18,19, the authors have errors.

We will correct them.




Assigned_Reviewer_8
==================================================================================================
Weakness (Cons)
1.	A good character segmentation is an important step required by the proposed approach. This issue limits the applicability of the overall framework in practice. However, the paper does not mention how the characters are segmented. I think it would be better for the readers if the paper discussed the limitations of the approach. 

Thanks for the advice, we will discuss the limitations of the approach. Many other works that focus on the representation also only included experiments on cropped images, and we aimed at presenting a representation that performs well for character recognition. 
--------------------------------------------------------------------------------------------------
