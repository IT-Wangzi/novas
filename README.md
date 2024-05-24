/*大论文里太阳位置获取示例：参考novas手册C61-C67*/
bool AstronomicalCalculation::CalcSunPox(UTC_TIME utc, Point3D &pos)
{
	short int error = 0;
	double ra = 0.;
	double dec = 0.;
	double dis = 0.;
	double JD_TT;
	Get_JD_TT(utc, JD_TT);
	cat_entry dummy_star;
	object sun;
	make_cat_entry("DUMMY", "xxx", 0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0,&dummy_star);
	if ((error = make_object(0, 10, "Sun", &dummy_star, &sun)) != 0)
	{
		printf("Error %d from make_object (Moon)\n", error);
		return false;
	}
	if ((error = app_planet(JD_TT, &sun, 0, &ra, &dec, &dis)) != 0)
	{
		printf("Error %d from app_planet.", error);
		return false;
	}
	sphD2cart<double>(ra*HOUR2DEG, dec, dis*AU, pos.x, pos.y, pos.z);
	return true;
 }
