## QIVISE: A Quantum-inspired Interactive Video Search Engine in VBS2023

This repo is the official implementation of "**QIVISE: A Quantum-inspired Interactive Video Search Engine in VBS2023**" by Weixi Song, Jiangshan He, Xinghan Li, Shiwei Feng, and Chao Liang.

This system participated in the VBS2023 and won the Best Newcomer. See details in [Video Browser Showdown](https://videobrowsershowdown.org/hall-of-fame/).

## Abstract

In this paper, we present a quantum-inspired interactive video search engine (QIVISE), which will be tested in VBS2023. QIVISE aims at assisting the user in dealing with Known-Item Search and Ad-hoc Video Search tasks with high efficiency and accuracy. QIVISE is based on a text-image encoder to achieve multi-modal embedding and introduces multiple interaction possibilities, including a novel quantum-inspired interaction on paradigm, label search, and multi-modal search to refine the retrieval results via user's interaction and feedback.  

## Approach

### Quantum-Inspired Interaction for Clips Re-ranking

Following the great leap in quantum-inspired information retrieval , our system introduces a novel quantum-inspired interaction paradigm to model the
user's interaction.\
After the first-round retrieval, the user can choose the shown video clips that are highly or not consistent with user's demands. With those chosen clips, we can get
an estimation about the user's true demands by applying the quantum-inspired method(see details in the paper). The user's true demands can be regarded as the space spanned by the chosen clip vector and its complement subspace, as shown in Fig.2.\
With the estimation of user's demand, the final score for re-ranking is defined as follow:
$$Score(v_ {eva})=cosine(q,v_ {eva})+\sum{cosine(r^ {i}_ {f},v_ {eva})}-\sum{cosine(r^ {i}_ {nf},v_ {eva})}$$
where $v_ {eva}$ is the video clip to be evaluated in the next-round, cosine(·) is cosine similarity, q is the query embedding vector, $r^ {i}_ {f}$ is the user's demand generated by the $i^ {th}$ highly consistent video and $r^ {i}_ {nf}$ is the user's demand estimation generated by the $i^ {th}$ not-consistent video.

![approach](https://github.com/song-wx/VBS-QIVISE/raw/main/assets/diagram.png#pic_center)


## VBS2023 Result

| Team          | KIS-T | KIS-V | AVS  | KIS-V-M |
| ------------- | ----- | ----- | ---- | ------- |
| HTW           | 1000  | 993   | 1000 | 1000    |
| VISIONE       | 943   | 1000  | 786  | 895     |
| VIREO         | 632   | 992   | 855  | 775     |
| vitrivr-VR    | 738   | 877   | 699  | 881     |
| CVHunter      | 727   | 596   | 855  | 848     |
| vitrivr       | 928   | 738   | 702  | 618     |
| Verge         | 513   | 845   | 775  | 667     |
| **QIVISE**    | 318   | 869   | 261  | 860     |
| VideoCLIP     | 267   | 540   | 561  | 488     |
| V-FIRST       | 708   | 700   | 363  | 0       |
| diveXplore    | 621   | 573   | 279  | 172     |
| 4MR           | 375   | 335   | 239  | 674     |
| PERFECT MATCH | 0     | 0     | 34   | 0       |

## Group Photo

![group_photo](assets/group_photo.jpg)

## Certificate of award 

![certificates_award](assets/VBS-Best%20Newcomer.jpg)

## Citation

If you find this repository useful, please consider giving ⭐ or citing: