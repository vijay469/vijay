<?php include('header.php'); ?>
<?php include('../../controller/users_controller.php'); ?>
<!--app-content open-->

<div class="app-content">
  <section class="section">
    <!--page-header open-->
    <div class="page-header">
      <ol class="breadcrumb">
        <!-- breadcrumb -->
        <li class="breadcrumb-item"><a href="dashboard"><i class="fe fe-home mr-2"></i>Dashboard</a></li>
        <li class="breadcrumb-item active" aria-current="page">Listening Task</li>
      </ol>
      <!-- End breadcrumb -->
    </div>
    <!--page-header closed-->
	
	<?php		
		$getid = $_GET['listning'];
		@$opt = $_GET['opt'];
		$query1=$funObj->getWritingtaskid($_GET['listning']);
		$row=mysqli_fetch_array($query1);
		
		$arrTitles=[];
		
		$que1 = mysqli_query($con,"select  id,file_title,question,answer_type,answer,correction,question_type from tbl_listening_questionoption  where exam_test=$getid group by answer_type order by 1 desc ;");
		$p=0;
		while($res1 = mysqli_fetch_array($que1)){ 
			$arrTitles[$res1['answer_type']]=$res1['file_title'];
		}
		
		$arrkeys=array_keys($arrTitles);
	?>
	
    <!--row open-->
    <div class="row">
      <div class="col-lg-12 col-xl-12 col-md-12 col-sm-12">
        <div class="card">
          <div class="card-header">
            <h4>General Listening</h4>
          </div>
          <div class="card-body">
            <form method="post" class="form-horizontal" enctype="multipart/form-data">
               <div class="row">
				<?php
				  if(!empty($opt)){
					  $que = mysqli_query($con,"select id,file_title,question,answer_type,answer,correction,question_type from tbl_listening_questionoption where exam_test='$getid' and answer_type='".$opt."' "); 
				  }else{
				 for($i=0;$i<count($arrkeys);$i++){
					  
					  $que = mysqli_query($con,"select id,file_title,question,answer_type,answer,correction,question_type from tbl_listening_questionoption where exam_test='$getid' and answer_type='".$arrkeys[$i]."' ");
				  }
				  }
				 $j=0;
					while($res = mysqli_fetch_array($que)){
					//	echo $res['question'];
						$iddd = $res['id'];
						$ansType = $res['answer_type'];
						$queType = $res['question_type'];
						$correctAns = $res['correction'];
						
						$fetchans = $res['answer'];
						$last_space_position = strrpos($fetchans, '+=');
						$fetchans = substr($fetchans, 0, $last_space_position);
						//echo $fetchans;

						$answer = explode('+=',$fetchans);
						$j++;
						if($ansType=='radio'){
				  ?>
				  
					<?php if($j==1){ echo '<div class="col-md-12"><p><strong>'.$res['file_title'].'</p></strong></div>'; } ?>
					
					<div class="col-md-6 clearfix">
					  <h3 style="font-size: 15px;"><?php echo '<strong>'.$j.'.</strong>&nbsp;&nbsp;'; echo$res['question']; ?></h3>
					  <ol style="list-style-type: upper-alpha;">
						<?php 
							foreach($answer as $ans){
								echo '<li><input type="radio" name="mcqradio[]" style="width: 20px;" /> '.$ans.' </li>';
							}
						?>
					  </ol> 
					  
					</div>
					<?php }elseif($ansType=='filltheBlanksType'){ ?>
					
					<?php if($j==1){ echo '<div class="col-md-12"><p><strong>'.$res['file_title'].'</p></strong></div>'; } ?>
					
					<div class="col-md-6">
					  <h3 style="font-size: 15px;"><?php echo '<strong>'.$j.'.</strong>&nbsp;&nbsp;'; echo$res['question']; ?></h3>
					   <?php 
							foreach($answer as $ans){
								echo '<input type="hidden" name="filltheblanktypes" value="'.$ans.'" />';
							}
						?>
					</div>
					
					<?php } ?>
					
                
				<?php } //While Loop ?>
                </div>
			  
            </form>
			
			 <ul class="pagination text-center">
                <!--li class="page-item"><a class="page-link" href="#">Previous</a></li-->
				<?php $p=0;foreach($arrTitles as $key=>$t){ ?>
					<li class="page-item <?php if($key==$opt)echo 'active'; ?> ">
						<a class="page-link" href="listening_answer?listning=<?php echo $getid.'&opt='.$key;?>"><?php echo ++$p;?></a>
					</li>
				<?php }?>
                <!--li class="page-item"><a type="submit" class="page-link" href="Clients.jsp">Next</a></li-->
            </ul>
			
          </div>
        </div>
      </div>
    </div>
    <!--row closed-->
  </section>
</div>
<!--app-content closed-->
<!-- Side-bar -->
<div class="sidebar sidebar-right sidebar-animate"> </div>
<!--side-bar Closed -->
<?php include('footer.php'); ?>
