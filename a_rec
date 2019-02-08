#-*- coding: utf-8 -*-

import random
import re
 
#获取用户-电影评分数据,格式为{(user,item):rating}
def genData_u_m_rating(M,k,seed):
    path_r = 'E:/datasets/ml-1m/ratings.dat'
    data_u_m_rating = dict()
    test = dict()   #获取测试集，格式为{(user,item):tating}
    random.seed(seed)
    with open(path_r,encoding = 'utf-8-sig') as file_r:
        for i in file_r:
            j = i.strip().split('::')
            if random.randint(0,M)==k:
                test[('u'+j[0],'m'+j[1])] = eval(j[2])
            else:
                data_u_m_rating[('u'+j[0],'m'+j[1])] = eval(j[2])
    print ("genData_u_m_rating successed!")
    return data_u_m_rating,test

#获取某个用户-电影元组中，电影标签的评分,格式为{(user,item):{mtag:rating}}
def getU_mtag_rating(data_u_m_rating,data_m_mt):
    data_u_mtag_rating = dict()
    for i,j in data_u_m_rating.items():
        data_u_mtag_rating[i] = dict()
        for k in data_m_mt[i[1]]:
            data_u_mtag_rating[i][k] = j
    print('getU_mtag_rating successed!')
    return data_u_mtag_rating

#获取用户标签,格式为{user:set(utag)}
def genData_u_ut(test):
    path_u = 'E:/datasets/ml-1m/users.dat'
    data_u_ut = dict()
    test_u_ut = dict()   #获取测试集中用户的用户标签
    test_user_set = set()
    for t,k in test:
        test_user_set.add(t)
    with open(path_u,encoding = 'utf-8-sig') as file_u:
        for i in file_u:
            j = i.strip().split('::')
            if 'u'+j[0] in test_user_set:
                test_u_ut['u'+j[0]] = set()
                for p in j[1:]:
                    test_u_ut['u'+j[0]].add(p)
            if 'u'+j[0] not in data_u_ut.keys():
                data_u_ut['u'+j[0]] = set()
            for p in j[1:]:
                data_u_ut['u'+j[0]].add(p)
    print('genData_u_ut successed!')
    return data_u_ut,test_u_ut

#获取电影的标签，格式为{item:set(mtag)}
def genData_m_mt():
    path_m = 'E:\datasets\ml-1m\movies.dat'
    split=re.compile('(\d\d\d\d)')
    data_m_mt = dict()
    with open(path_m,encoding = 'utf-8-sig') as file_m:
        for i in file_m:
            j = i.strip().split('::')
            result=split.search(j[1]).group()
            j[1] = str(result)
            j[2] = j[2].split('|')
            for l in j[2]:
                j.append(l)
            j.remove(j[2])
            data_m_mt['m'+j[0]] = set()
            for p in j[1:]:
                data_m_mt['m'+j[0]].add(p)
    print('genData_m_mt successed!')
    return data_m_mt

#获取用户-电影标签，格式为{user:{mtag:count}}
def getData_u_mt(data_m_mt,data_u_m_rating):
    data_u_mt = dict()
    for i,j in data_u_m_rating:
        if i not in data_u_mt.keys():
            data_u_mt[i] = {j:1}
        if j in data_m_mt.keys():
            for k in data_m_mt[j]:
                if k not in data_u_mt[i].keys():
                    data_u_mt[i][k] = 1
                else:
                    data_u_mt[i][k] += 1
    print('genData_u_mt successed!')
    return data_u_mt

#获取用户所有标签，格式为{user:{tag:count}}
def getU_tags(data_u_ut,data_u_mt):
    data_u_tags = data_u_mt
    for i,j in data_u_ut.items():
        if i not in data_u_tags.keys():
            data_u_tags[i] = dict()
        for k in j:
            data_u_tags[i][k] = 1
    print('getU_tags successed!')
    return data_u_tags

#获取观看某电影的用户的所有标签,格式为{item:{tags:count}}
def getM_tags(data_m_mt,data_u_ut,data_u_m_rating):
    data_m_tags = dict()
    for u,i in data_u_m_rating:
        if i not in data_m_tags.keys():
            data_m_tags[i] = dict()
        for k in data_m_mt[i]:
            data_m_tags[i][k] = 1
        for l in data_u_ut[u]:
            if l not in data_m_tags[i].keys():
                data_m_tags[i][l] = 1
            else:
                data_m_tags[i][l] += 1
    print('getM_tags successed!')
    return data_m_tags

#获取用户标签总数,格式为{user:count_of_all_tags}
def getU_tag_all(data_u_tags):
    data_u_tag_all = dict()
    for i,j in data_u_tags.items():
        if i not in data_u_tag_all.keys():
            data_u_tag_all[i] = 0
        for k,l in j.items():
            data_u_tag_all[i] += l
    print('getU_tag_all successed!')
    return data_u_tag_all

#获取电影的电影标签总数,格式为{item:count_of_all_mtag}
def getM_tag_all(data_m_tags):
    data_m_tag_all = dict()
    for i,j in data_m_tags.items():
        if i not in data_m_tag_all.keys():
            data_m_tag_all[i] = 0
        for k,l in j.items():
            data_m_tag_all[i] += 1
    print('getM_mtag_all successed!')
    return data_m_tag_all

#获取每位用户对标签评价的总和，格式为{user:mtag_rating_sum}
def getSum_rating_u_mt(data_u_mtag_rating):
    sum_rating_u_mt = dict()
    for i,j in data_u_mtag_rating.items():
        if i[0] not in sum_rating_u_mt.keys():
            sum_rating_u_mt[i[0]] = 0
        for k,l in j.items():
            sum_rating_u_mt[i[0]] += l
    return sum_rating_u_mt

#获取用户所有评分之和，格式为{user:sum_rating}
def getSum_rating_u_m(data_u_m_rating):
    sum_rating_u_m = dict()
    for i,j in data_u_m_rating.items():
        if i[0] not in sum_rating_u_m.keys():
            sum_rating_u_m[i[0]] = 0
        sum_rating_u_m[i[0]] += j
    print('getSum_rating_u_m successed!')
    return sum_rating_u_m

#获取电影所有评分之和，格式为{item:sum_rating}
def getSum_rating_m(data_u_m_rating):
    sum_rating_m = dict()
    for i,j in data_u_m_rating.items():
        if i[1] not in sum_rating_m.keys():
            sum_rating_m[i[1]] = 0
        sum_rating_m[i[1]] += j
    print('getSum_rating_m successed!')
    return sum_rating_m

#获取对应标签所有评分之和，格式为{mtag:sum_rating}
def getSum_rating_ut(data_u_mtag_rating):
    sum_rating_mt = dict()
    for i,j in data_u_mtag_rating.items():
        for k,l in j.items():
            if k not in sum_rating_mt.keys():
                sum_rating_mt[k] = 0
            sum_rating_mt[k] += l
    print('getSum_rating_ut successed!')
    return sum_rating_mt

#获取每个用户或电影标签链接的用户或电影,格式为{tag:set(u_or_i)}
def getT_ui(data_ui_t):
    data_t_ui = dict()
    for i,j in data_ui_t.items():
        for k in j:
            if k not in data_t_ui.keys():
                data_t_ui[k] = set()
            data_t_ui[k].add(i)
    return data_t_ui

#获取每个用户标签链接的电影
def getUt_m(data_u_ut,data_u_m_rating):
    data_ut_m = dict()
    for i,j in data_u_m_rating:
        for k in data_u_ut[i]:
            if k not in data_ut_m.keys():
                data_ut_m[k] = set()
            data_ut_m[k].add(j)
    print('getUt_m successed!')
    return data_ut_m

#获取每个电影标签链接的用户,格式为{mtag:set(user)}
def getMt_u(data_u_mt):
    data_mt_u = dict()
    for i,j in data_u_mt.items():
        for k in j:
            if k not in data_mt_u.keys():
                data_mt_u[k] = set()
            data_mt_u[k].add(i)
    print('getMt_u successed!')
    return data_mt_u

#获取每位用户对用户标签的权值，格式为{utag:{user:1/n}}
def getValue_ut_u(data):
    value_ut_u = dict()
    for i,j in data.items():
        if i not in value_ut_u.keys():
            value_ut_u[i] = dict()
        for k in j:
            value_ut_u[i][k] = 1/len(j)
    print('getValue_ut_u of utag successed!')
    return value_ut_u    
    
#获取每部电影对电影标签的权值，格式为{utag:{user:1/n}}
def getValue_mt_m(data):
    value_mt_m = dict()
    for i,j in data.items():
        if i not in value_mt_m.keys():
            value_mt_m[i] = dict()
        for k in j:
            value_mt_m[i][k] = 1/len(j)
    print('getValue_umt_ui of mtag successed!')
    return value_mt_m

#获取标签对相应主体的权值
def getValue_u_ut(data_u_ut):
    value_u_ut = dict()
    for i,j in data_u_ut.items():
        if i not in value_u_ut.keys():
            value_u_ut[i] = dict()
        for k in j:
            value_u_ut[i][k] = 1/len(j)
    return value_u_ut

def getValue_m_mt(data_m_mt):
    value_m_mt = dict()
    for i,j in data_m_mt.items():
        if i not in value_m_mt.keys():
            value_m_mt[i] = dict()
        for k in j:
            value_m_mt[i][k] = 1/len(j)
    return value_m_mt

#获取每部电影对用户的权值，格式为{user:{item:rating/sum_rating_u_m}}
def getValue_u_m(data_u_m_rating,sum_rating_u):
    value_u_m = dict()
    for i,j in data_u_m_rating.items():
        if i[0] not in value_u_m.keys():
            value_u_m[i[0]] = dict()
        value_u_m[i[0]][i[1]] = j/sum_rating_u[i[0]]
    print('getValue_u_m successed!')
    return value_u_m
    
#获取每位用户对电影的权值，格式为{item:{user:rating/sum_rating_m}}
def getValue_m_u(data_u_m_rating,sum_rating_m):
    value_m_u = dict()
    for i,j in data_u_m_rating.items():
        if i[1] not in value_m_u.keys():
            value_m_u[i[1]] = dict()
        value_m_u[i[1]][i[0]] = j/sum_rating_m[i[1]]
    print('getValue_m_u successed!')
    return value_m_u

#获取每个电影标签对用户的权值，格式为{user:{mtag:rating/sum_rating_u}}
def getValue_u_mt(data_u_mtag_rating,sum_rating_u_mt):
    value_u_mt = dict()
    for i,j in data_u_mtag_rating.items():
        if i[0] not in value_u_mt.keys():
            value_u_mt[i[0]] = dict()
        for k,l in j.items():
            if k not in value_u_mt[i[0]].keys():
                value_u_mt[i[0]][k] = 0
            value_u_mt[i[0]][k] += l/sum_rating_u_mt[i[0]]
    print('getValue_u_mt successed!')
    return value_u_mt

#获取每位用户对电影标签的权值，格式为{mtag:{user:rating/sum_rating_mt}}
def getValue_mt_u(data_u_mtag_rating,sum_rating_mt):
    value_mt_u = dict()
    for i,j in data_u_mtag_rating.items():
        for k,l in j.items():
            if k not in value_mt_u.keys():
                value_mt_u[k] = dict()
            if i[0] not in value_mt_u[k].keys():
                value_mt_u[k][i[0]] = 0
            value_mt_u[k][i[0]] += l/sum_rating_mt[k]
    print('getValue_mt_u successed!')
    return value_mt_u

#获取ut的邻接mt，格式为{utag:{mtag:value}}
def getUt_u_mt(value_ut_u,value_u_mt):
    next_ut_u_mt = dict()   #{utag:{mtag:value}}
    for i,j in value_ut_u.items():
        if i not in next_ut_u_mt.keys():
            next_ut_u_mt[i] = dict()
        next_ut_u_mt[i] = getNext_mt(j,value_u_mt)
    print('getUt_u_mt successed!')
    return next_ut_u_mt

#获取ut通过链ut-u-m-mt链接的mt，格式为{utag:{mtag:value}}
def getUt_u_m_mt(value_ut_u,value_u_m,value_m_mt):
    next_ut_u_m_mt = dict()
    dic1 = dict()
    for i,j in value_ut_u.items():
        if i not in next_ut_u_mt.keys():
            next_ut_u_mt[i] = dict()
        dic1[i] = dict()
        dic1[i] = getNext_mt(j,value_u_m)   #获取链接的电影,格式为{item:1/n * 1/n}
        next_ut_u_m_mt[i] = getNext_mt(dic1[i],value_m_mt)
    print('getUt_u_m_mt successed!')
    return next_ut_u_m_mt

#获取与ut相链接的mt及其值，格式为{mtag:1/n * rating/sum_rating_u_mt}
def getNext_mt(data,value):
    dic1 = dict()
    for i,j in data.items():
        for k,l in value[i].items():
            if k not in dic1.keys():
                dic1[k] = 0
            dic1[k] += j*l
    return dic1

#mt通过路径mt_u_ut链接的ut，格式为{mtag:{utag:value}} 
def getMt_u_ut(value_mt_u,value_u_ut):
    next_mt_u_ut = dict()   #{utag:{mtag:value}}
    for i,j in value_mt_u.items():
        if i not in next_mt_u_ut.keys():
            next_mt_u_ut[i] = dict()
        next_mt_u_ut[i] = getNext_ut(j,value_u_ut)
    print('getMt_u_ut successed!')
    return next_mt_u_ut

#mt通过路径mt_m_u_ut链接的ut，格式为{mtag:{utag:value}}  
def getMt_m_u_ut(value_mt_m,value_m_u,value_u_ut):
    next_mt_m_u_ut = dict()
    dic1 = dict()
    for i,j in value_mt_m.items():
        if i not in next_mt_m_u_ut.keys():
            next_mt_m_u_ut[i] = dict()
        dic1[i] = dict()
        dic1[i] = getNext_ut(j,value_m_u)   #获取链接的电影,格式为{item:1/n * 1/n}
        next_mt_m_u_ut[i] = getNext_ut(dic1[i],value_u_ut)
    print('getMt_m_u_ut successed!')
    return next_mt_m_u_ut

#获取与ut相链接的mt及其值，格式为{mtag:1/n * rating/sum_rating_u_mt}
def getNext_ut(data,value):
    dic1 = dict()
    for i,j in data.items():
        if i not in value.keys():
            continue
        for k,l in value[i].items():
            if k not in dic1.keys():
                dic1[k] = 0
            dic1[k] += j*l
    return dic1

def Mixing(data1,data2,alpha):
    Linear_Mixing = dict()
    for i,j in data1.items():
        if i not in Linear_Mixing.keys():
            Linear_Mixing[i] = dict()
        if i not in data2.keys():
            continue
        for p,q in j.items():
            if p not in data2[i].keys():
                Linear_Mixing[i][p] = q*alpha
                continue
            for k,l in data2[i].items():
                if k not in j.keys():
                    Linear_Mixing[i][k] = l*(1-alpha)
                    continue
                Linear_Mixing[i][k] = q*alpha+l*(1-alpha)
    return Linear_Mixing

#以参数beta进行筛选，得到结果，格式为[set(tag)]
def getClub(Linear_Mixing_ut_mt,Linear_Mixing_mt_ut,beta):
    club = []
    for i,j in Linear_Mixing_ut_mt.items():
        se1 = set()
        se1.add(i)
        for k,l in j.items():
            if k not in Linear_Mixing_mt_ut.keys():
                continue
            if l > beta:
                se1.add(k)
            for p,q in Linear_Mixing_mt_ut[k].items():
                if q > beta:
                    se1.add(p)
        club.append(frozenset(se1))
    for m in club:
        for n in club:
            if len(m-n) == 0:
                club.remove(n)
    print('getClub successed!')
    return club

#计算每个tag归属多少个社团，格式为{tag:count}
def getTag_contri_count(club):
    tag_contri_count = dict()
    for i in club:
        for j in i:
            if j not in tag_contri_count.keys():
                tag_contri_count[j] = 0
            tag_contri_count[j] += 1
    print('getTag_contri_count successed!')
    return tag_contri_count

#计算每个社团中tag的归属度，格式为{frozenset():{tag:contribution}}
def getClub_tag_contri(club,tag_contri_count):
    club_tag_contri = dict()
    for i in club:
        club_tag_contri[i] = dict()
        for j in i:
            club_tag_contri[i][j] = 1/tag_contri_count[j]
    print('getClub_tag_contri successed!')
    return club_tag_contri

#计算每个tag对其所属的各个社团的归属度，格式为{tag:{frozenset():contribution}}
def getTag_club_contri(club_tag_contri):
    tag_club_contri = dict()
    for i,j in club_tag_contri.items():
        for k,l in j.items():
            if k not in tag_club_contri.keys():
                tag_club_contri[k] = dict()
            tag_club_contri[k][i] = l
    print('getTag_club_contri successed!')
    return tag_club_contri

#获取各个电影的归属社团，格式为{item:{club:contribution}}
def getItem_club_contri(club,value_mt_m):
    item_club_contri = dict()
    for i in club:
        for j in i:
            if j in value_mt_m.keys():
                for k,l in value_mt_m[j].items():
                    if k not in item_club_contri.keys():
                        item_club_contri[k] = dict()
                    if i not in item_club_contri[k].keys():
                        item_club_contri[k][i] = 0
                    item_club_contri[k][i] += l
    print('getItem_club_contri successed!')
    return item_club_contri

def getClub_item_contri(item_club_contri):
    club_item_contri = dict()
    for i,j in item_club_contri.items():
        for k,l in j.items():
            if k not in club_item_contri.keys():
                club_item_contri[k] = dict()
            club_item_contri[k][i] = l
    print('getClub_item_contri successed!')
    return club_item_contri

def evaluate(test, test_u_ut, club_tag_contri, tag_club_contri, club_item_contri,N):
    '''
    :param test: 测试集
    :param N: TopN推荐中N数目
    :return:返回计算的值率
    '''
    test_u_m = dict()
    for i,j in test:
        if i not in test_u_m.keys():
            test_u_m[i] = set()
        test_u_m[i].add(j)
    recall = getRecall(test_u_m,test_u_ut,tag_club_contri,club_item_contri,N)
    precision = getPrecision(test_u_m,test_u_ut,tag_club_contri,club_item_contri,N)
    return recall, precision

#计算查全率
def getRecall(test,test_u_ut,tag_club_contri,club_item_contri,N):
    recall = 0
    hit=0# 预测准确的数目
    totla=0# 所有行为总数
    for i,j in test_u_ut.items():
        rank = getRecommendation(test_u_ut[i],tag_club_contri,club_item_contri,N)
        for k in rank:
            if k in test[i]:
                hit += 1
        totla += len(test[i])
    recall = hit/(totla*1.0)
    print('getRecall successed!')
    return recall

#计算查准率
def getPrecision(test,test_u_ut,tag_club_contri,club_item_contri,N):
    precision = 0
    hit=0# 预测准确的数目
    totla=0# 所有行为总数
    for i,j in test_u_ut.items():
        rank = getRecommendation(test_u_ut[i],tag_club_contri,club_item_contri,N)
        for k in rank:
            if k in test[i]:
                hit += 1
        totla += N
    precision = hit/(totla*1.0)
    print('getPrecision successed!')
    return precision

#{item:value}
def getRecommendation(test_u_ut_set,tag_club_contri,club_item_contri,N):
    #{club:value}
    recom_club = dict()
    recom_item = dict()
    items = []
    sigma = 0.001
    for i in test_u_ut_set:
        if i in tag_club_contri.keys():
            for k,l in tag_club_contri[i].items():
                if l>sigma:
                    if k not in recom_club.keys():
                        recom_club[k] = 0
                    recom_club[k] += l
    for p,q in recom_club.items():
        for m,n in club_item_contri[p].items():
            if m not in recom_item.keys():
                recom_item[m] = 0
            if n>sigma:
                recom_item[m] += q*n
       # print(recom_item)
    li = sorted(recom_item.items(), key=lambda x: x[1], reverse=True)
    count = 0
    for i in li:
        if count<N:
            items.append(i[0])
    return items

if __name__=='__main__':

    '''
#    划分训练集和测试集
#    :param data:传入的数据
#    :param M:测试集占比
#    :param k:一个任意的数字，用来随机筛选测试集和训练集
#    :param seed:随机数种子，在seed一样的情况下，其产生的随机数不变
#    :return:train:训练集 test：测试集，都是列表，元素是(user,item)元组
    '''
    M = 10
    k = 5
    seed = 10
    #获取用户-电影评分数据,格式为{(user,item):rating}
    (data_u_m_rating,test) = genData_u_m_rating(M,k,seed)
    #获取用户标签,格式为{user:set(utag)}
    (data_u_ut,test_u_ut) = genData_u_ut(test)
    #获取电影的标签，格式为{item:set(mtag)}
    data_m_mt = genData_m_mt()
    #获取用户-电影标签，格式为{user:{mtag:count}}
    data_u_mt = getData_u_mt(data_m_mt,data_u_m_rating)
    #获取每个用户标签链接的用户,格式为{utag:set(user)}
    data_ut_u = getT_ui(data_u_ut)
    print('getT_ui of user successed!')
    #获取每个用户标签链接的电影,格式为{utag:set(item)}
    data_ut_m = getUt_m(data_u_ut,data_u_m_rating)
    #获取每个电影标签链接的用户,格式为{mtag:set(user)}
    data_mt_u = getMt_u(data_u_mt)
    #获取每个用户标签链接的用户,格式为{mtag:set(item)}
    data_mt_m = getT_ui(data_m_mt)
    print('getT_ui of item successed!')
    #获取每位用户观看电影的标签的评分,格式为{(user,item):{mtag:rating}}
    data_u_mtag_rating = getU_mtag_rating(data_u_m_rating,data_m_mt)
    #获取用户所有标签，格式为{user:{tag:count}}
    data_u_tags = getU_tags(data_u_ut,data_u_mt)
    #获取观看某电影的用户的所有标签,格式为{item:{tags:count}}
    data_m_tags = getM_tags(data_m_mt,data_u_ut,data_u_m_rating)
    #获取用户标签总数,格式为{user:count_of_all_tags}
    data_u_tag_all = getU_tag_all(data_u_tags)
    #获取电影的电影标签总数,格式为{item:count_of_all_mtag}
    data_m_tag_all = getM_tag_all(data_m_tags)
    #获取用户所有评分之和，格式为{user:sum_rating}
    sum_rating_u_m = getSum_rating_u_m(data_u_m_rating)
    #获取电影所有评分之和，格式为{item:sum_rating}
    sum_rating_m = getSum_rating_m(data_u_m_rating)
    #获取每位用户对标签评价的总和，格式为{user:mtag_rating_sum}
    sum_rating_u_mt = getSum_rating_u_mt(data_u_mtag_rating)
    #获取对应标签所有评分之和，格式为{mtag:sum_rating}
    sum_rating_mt = getSum_rating_ut(data_u_mtag_rating)
    #获取每部电影对用户的权值，格式为{user:{item:rating/sum_rating_u_m}}
    value_u_m = getValue_u_m(data_u_m_rating,sum_rating_u_m)
    #获取每位用户对电影的权值，格式为{item:{item:rating/sum_rating_m}}
    value_m_u = getValue_m_u(data_u_m_rating,sum_rating_m)
    #获取每个电影标签对用户的权值，格式为{user:{mtag:rating/sum_rating_u_mt}}
    value_u_mt = getValue_u_mt(data_u_mtag_rating,sum_rating_u_mt)
    #获取每位用户对电影标签的权值，格式为{mtag:{user:rating/sum_rating_mt}}
    value_mt_u = getValue_mt_u(data_u_mtag_rating,sum_rating_mt)
    #获取每个用户标签对用户的权值，格式为{user:{utag:1/n}}
    value_u_ut = getValue_u_ut(data_u_ut)
    print('getValue_u_ut of user successed!')
    #获取每个电影标签对电影的权值，格式为{item:{mtag:1/n}}
    value_m_mt = getValue_m_mt(data_m_mt)
    print('getValue_m_mt of user successed!')
    #获取每位用户对用户标签的权值，格式为{utag:{user:1/n}}
    value_ut_u = getValue_ut_u(data_ut_u)
    #获取每部电影对电影标签的权值，格式为{mtag:{item:1/n}}
    value_mt_m = getValue_mt_m(data_mt_m)
    #ut通过路径ut_u_mt链接的mt，格式为{utag:{mtag:value}}
    next_ut_u_mt = getUt_u_mt(value_ut_u,value_u_mt)
    #ut通过路径ut_u_m_mt链接的mt，格式为{utag:{mtag:value}}
    next_ut_u_m_mt = getUt_u_m_mt(value_ut_u,value_u_m,value_m_mt)
    #mt通过路径mt_u_ut链接的ut，格式为{mtag:{utag:value}}
    next_mt_u_ut = getMt_u_ut(value_mt_u,value_u_ut)
    #mt通过路径mt_m_u_ut链接的ut，格式为{mtag:{utag:value}}
    next_mt_m_u_ut = getMt_m_u_ut(value_mt_m,value_m_u,value_u_ut)
    
    alpha = 0.3
    beta = 0.01
    #以参数alpha对经过两条路径获得的结果进行线性混合，格式为{utag:{mtag:value}}
    Linear_Mixing_ut_mt = Mixing(next_ut_u_mt,next_ut_u_m_mt,alpha)
    print('Mixing for Linear_Mixing_ut_mt successed!')
    #以参数alpha对经过两条路径获得的结果进行线性混合，格式为{mtag:{utag:value}}
    Linear_Mixing_mt_ut = Mixing(next_mt_u_ut,next_mt_m_u_ut,alpha)
    print('Mixing for Linear_Mixing_mt_ut successed!')
    #以参数beta进行筛选，获得以frozenset格式为元素的列表，格式为{frozenset(tag)}
    club = getClub(Linear_Mixing_ut_mt,Linear_Mixing_mt_ut,beta)
    #计算每个tag归属多少个社团，格式为{tag:club_count}
    tag_contri_count = getTag_contri_count(club)
    #计算每个社团中tag的归属度，格式为{club:{tag:contribution}}
    club_tag_contri = getClub_tag_contri(club,tag_contri_count)
    #计算每个tag对其所属的各个社团的归属度，格式为{tag:{club:contribution}}
    tag_club_contri = getTag_club_contri(club_tag_contri)
    #获取各个电影的归属社团，格式为{item:{club:contribution}}
    item_club_contri = getItem_club_contri(club,value_mt_m)   #注，对各个社团的归属度总和大于1
    #获取各个电影对其所属社团的归属度，格式为{club:{item:contribution}}
    club_item_contri = getClub_item_contri(item_club_contri)
    
    N = 20
    recall, precision=evaluate(test, test_u_ut, club_tag_contri, tag_club_contri, club_item_contri,N)
    print('recall:',recall)
    print('precision:',precision)
